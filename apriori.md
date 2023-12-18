# **Understanding the Apriori Algorithm**
---

The Apriori algorithm,  introduced by R. Agrawal and R. Srikant in 1994, is a classic algorithm in data mining used for mining frequent itemsets and discovering association rules in a database. This page aims to provide an overview of the Apriori algorithm, its method, key parameters, advantages, limitations, and a basic implementation guide using Python.

**${\color{yellow}\textsf{METHOD}}$**
---
The Apriori algorithm operates by identifying frequent itemsets in a dataset and then extending them to larger itemsets, provided they appear sufficiently frequently in the database. It explores the itemset space in a breadth-first manner. Initially, it identifies frequent itemsets of size 1, then uses these to generate candidate itemsets of size 2, and so on, progressively increasing the size of itemsets and checking their support.

>[!Note]
>A key principle of Apriori is that **${\color{yellow}\textsf{a subset of a frequent itemset must also be frequent}}$**. For instance, if the itemset {A, B, C} is frequent, it implies that all of its subsets, such as {A, B}, {A, C}, {B, C}, {A}, {B}, and {C}, must also be frequent. This is because any transaction containing {A, B, C} also contains all its subsets. This principle efficiently reduces the number of support calculations needed and speeds up the discovery of association rules in large datasets.

![image](https://github.com/andytoh78/market-basket-analysis/assets/139482827/7a687193-0eca-46e9-a34d-9b583466c3af)

&nbsp;

**${\color{yellow}\textsf{KEY PARAMETERS}}$**
---
| **Parameter**             | **Description**                                                               |
|:--------------------------|:------------------------------------------------------------------------------|
| `min_support`             | This is the user-defined threshold (a decimal between 0 and 1) that determines the minimum frequency at which an itemset must be present in the dataset to be considered 'frequent'.|
| `confidence_threshold`    | This is the user-defined minimum level of confidence that an association rule must exceed to be considered significant.
| `lift_threshold`          | This is the user-defined minimum lift value (typically >1) focusing on rules that have a positive relationship between the antecedent and consequent.|

&nbsp;

**${\color{yellow}\textsf{PROS | CONS}}$**
---
| **Pros**                                          | **Cons**                                                    |
|:--------------------------------------------------|:------------------------------------------------------------|
| Simple to understand and easy to implement.      | Inefficient for large datasets due to multiple database scans and generation of numerous candidate sets.
| Effective for small to medium-sized datasets.      |                                                             |

&nbsp;

**${\color{yellow}\textsf{IMPLEMENTATION STEPS}}$**
---
We will use [Market_Basket_Optimisation](https://github.com/username/repository/blob/branch/path/to/Market_Basket_Optimisation.csv) to demonstrate the application of Apriori algorithm in Market Basket Analysis

```python
# Install machine learning extensions (mlxtend) library
pip install mlxtend
```
```python
# Load dataset and view first 5 rows
import numpy as np
import pandas as pd
pd.set_option("display.max_columns", None)
pd.set_option("display.max_colwidth", None)

df = pd.read_csv("/kaggle/input/market-basket-optimisation/Market_Basket_Optimisation.csv", header=None, index_col=None, names=[f"Item_{i}" for i in range(1, 21)])
df.head()
```
Below shows the first five rows of the dataset, where each row represents a basket or collection of items that are purchased together in a single transaction.<br><br>
![image](https://github.com/andytoh78/market-basket-analysis/assets/139482827/edeccbd7-a940-4841-b189-2dd55f2397c9)

```python
# View shape of the dataset
print(f"The dataset contains {df.shape[0]} transactions and each transaction has a maximum of {df.shape[1]} items or less.")
```
The dataset contains 7501 transactions and each transaction has a maximum of 20 items or less.

```python
# Generate transaction lists
txns = df.fillna("").values.tolist()
txns = [[item for item in txn if item != ''] for txn in txns]
txns = [[item.strip() for item in txn] for txn in txns]

# Create a list of unique ids for the transactions
ids = [i + 1 for i in range(len(txns))]

# Initialize an empty list
data =[]
# Iterate through transactions and add them to the DataFrame with IDs
for i, txn in enumerate(txns):
    data.extend([{'TID': ids[i], 'Item': item} for item in txn])

df_txn = pd.DataFrame(data)
df_txn.head(25)
```
| TID  | Item              | 
|:---  |:----------------- | 
| 1    | shrimp            |
| 1    | almonds           |
| 1    | avocado           |
| 1    | vegetables mix    |
| 1    | green grapes      |
| 1    | whole weat flour  |
| 1    | yams              |
| 1    | cottage cheese    |
| 1    | energy drink      |
| 1    | tomato juice      |
| 1    | low fat yogurt    |
| 1    | green tea         |
| 1    | honey             |
| 1    | salad             |
| 1    | mineral water     |
| 1    | salmon            |
| 1    | antioxydant juice |
| 1    | frozen smoothie   |
| 1    | spinach           |
| 1    | olive oil         |
| 2    | burgers           |
| 2    | meatballs         |
| 2    | eggs              |
| 3    | chutney           |
| 4    | turkey            |

```python
# Perform one hot encoding using TransactionEncoder
from mlxtend.preprocessing import TransactionEncoder

# Create a TransactionEncoder
te = TransactionEncoder()

# Fit and transform the transaction data
te_array = te.fit(txns).transform(txns)

# Extract the column names
te_columns = te.columns_

# Create a DataFrame from the one-hot encoded array
df1 = pd.DataFrame(te_array, columns=te.columns_)

# Display the results
df1.head()
```
![image](https://github.com/andytoh78/market-basket-analysis/assets/139482827/01e3cc0c-b119-4823-9edb-a6fc8d21a0df)

The output of the above code snippet provides a binary representation of the transaction data. Each row represents a transaction, and each column corresponds to an item. If an item is present in a transaction, the corresponding cell value will be indicated as `True`; otherwise, it will be `False`. This **${\color{yellow}\textsf{one-hot encoded}}$** format is commonly used for various data mining tasks, including association rule mining. This is also the required format when using the Apriori alogrithm in identifying frequent itemsets and association rules.

Before applying the Apriori algorithm, let's first take a look at the 30 most commonly purchased items in the dataset

```python
# Find the top 30 most frequent items
top_30items = df_txn['Item'].value_counts().head(30).reset_index()

# Convert the top 30 items into DataFrame and sort by item count in descending order
df_top_30items = pd.DataFrame(top_30items)
df_top_30items.columns = ['Item', 'Count']

# Calculate the percentage of transactions for each item
total_transactions = len(df)
df_top_30items['% Count'] = (df_top_30items['Count']*100 / total_transactions).round(2)

# Display the results
df_top_30items
```
| Item             | Count | % Count |
|:-----------------|:------|:--------|
| mineral water    | 1788  | 23.84   |
| eggs             | 1348  | 17.97   |
| spaghetti        | 1306  | 17.41   |
| french fries     | 1282  | 17.09   |
| chocolate        | 1230  | 16.40   |
| green tea        | 991   | 13.21   |
| milk             | 972   | 12.96   |
| ground beef      | 737   | 9.83    |
| frozen vegetables| 715   | 9.53    |
| pancakes         | 713   | 9.51    |
| burgers          | 654   | 8.72    |
| cake             | 608   | 8.11    |
| cookies          | 603   | 8.04    |
| escalope         | 595   | 7.93    |
| low fat yogurt   | 574   | 7.65    |
| shrimp           | 536   | 7.15    |
| tomatoes         | 513   | 6.84    |
| olive oil        | 494   | 6.59    |
| frozen smoothie  | 475   | 6.33    |
| turkey           | 469   | 6.25    |
| chicken          | 450   | 6.00    |
| whole wheat rice | 439   | 5.85    |
| grated cheese    | 393   | 5.24    |
| cooking oil      | 383   | 5.11    |
| soup             | 379   | 5.05    |
| herb & pepper    | 371   | 4.95    |
| honey            | 356   | 4.75    |
| champagne        | 351   | 4.68    |
| fresh bread      | 323   | 4.31    |
| salmon           | 319   | 4.25    |

Mineral water is the most frequently purchased item, and it appears in 1788 (~24%) transactions. Several food items like eggs, spaghetti, french fries, chocolate, and green tea also have high purchase counts. We can also visualize the frequent items using bar charts, heatmaps, pie charts, tree maps, word cloud, network graph to better understand their distribution within the dataset.


### **${\color{lightgreen}\textsf{Bar Plot}}$**
```python
# Create the countplot with the filtered data
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(18, 6))
plt.bar(data=df_top_30items, x="Item", height="Count", edgecolor="black", color="skyblue")
ax.bar_label(ax.containers[0], fontsize=10, fontweight=600)
plt.title("Top 30 Most Frequent Items\n", fontsize=14, fontweight=700)
plt.xlabel("\nItem", fontsize=12, fontweight=700)
plt.ylabel("Quantity\n", fontsize=12, fontweight=700)
plt.xticks(rotation=90)
plt.gca().grid(False)
plt.gca().spines[["left", "bottom"]].set_color("black")
plt.gca().spines[["right", "top"]].set_visible(False)
plt.gca().set_facecolor("white")
plt.tight_layout()
plt.show()
```
![image](https://github.com/andytoh78/market-basket-analysis/assets/139482827/64d9f897-98e6-4310-b569-8c2c85105dbf)

### **${\color{lightgreen}\textsf{Word Cloud}}$**
A word cloud can be an engaging way to visualize frequent items where the size of each item's name is proportional to its frequency. This can be useful for a quick view and identification of the most common items, although it is less precise in terms of quantification.
```python
from wordcloud import WordCloud

# Combine Item values into a string with space separator
all_values = [item for txn in txns for item in txn]
all_values_list = ' '.join(all_values)

# Create a word cloud object
wordcloud = WordCloud(width=300, height=300, background_color="white", min_font_size=8, colormap='viridis').generate(all_values_list)

# Display the word cloud
plt.imshow(wordcloud, interpolation="bilinear")
plt.axis("off")
plt.tight_layout(pad=0)
plt.show()
```
<img src="https://github.com/andytoh78/market-basket-analysis/assets/139482827/83be14e4-893d-42c8-932b-0aa34ea84e98" width="400" height="400">

### **${\color{lightgreen}\textsf{Tree Map}}$**
Tree maps display each item as a rectangle, with the size corresponding to the frequency of the item. This can be a visually appealing way to represent hierarchical data and show relative patterns at a glance.
```python
import squarify

# Convert 'Count' column to integers
df_top_30items['Count'] = df_top_30items['Count'].astype(int)

# Create a figure and axis
fig, ax = plt.subplots(figsize=(12, 6))

# Calculate treemap sizes
sizes = df_top_30items['Count']

# Calculate treemap labels
labels = [f"{item}\n({count})" for item, count in zip(df_top_30items['Item'], df_top_30items['Count'])]

# Plot the treemap
squarify.plot(sizes=sizes, label=labels, alpha=0.8, color=sns.color_palette("Spectral", len(df_top_30items)), text_kwargs={'fontsize':10}, ax=ax)

plt.axis('off')
plt.title("Tree Map for Top 30 Most Frequent Items\n", fontsize=14, fontweight=700)
plt.show()
```
<img src="https://github.com/andytoh78/market-basket-analysis/assets/139482827/32b713fd-38f6-4dec-ab21-f481cf8f3122" width="700" height="400">

