# **Eclat Algorithm**
---

The Eclat (Equivalence Class Transformation) algorithm is another classic data mining algorithm used for mining frequent itemsets and discovering association rules in a database. It differs from the Apriori algorithm in terms of its methodology and efficiency. This page aims to provide an overview of the Eclat algorithm, its method, key parameters, advantages, limitations, and a basic implementation guide using Python.

**${\color{yellow}\textsf{METHOD}}$**
---
The Eclat algorithm employs a depth-first search strategy to find frequent itemsets in a dataset. Instead of generating candidate itemsets as in Apriori, Eclat uses a **${\color{yellow}\textsf{vertical data format to represent transactions}}$**. It maintains an index structure, often called the **${\color{yellow}\textsf{tidset}}$**, which records the transactions in which each item appears. Eclat then recursively combines frequent itemsets by **${\color{yellow}\textsf{intersecting their tidsets}}$**. This approach scans the database only once, eliminates the need for candidate generation, making it efficient for mining frequent itemsets in large databases.

![image](https://github.com/andytoh78/market-basket-analysis/assets/139482827/8be5e469-df60-4b25-84e4-8d00f583fb31)

&nbsp;

**${\color{yellow}\textsf{KEY PARAMETERS}}$**
---
| **Parameter**             | **Description**                                                               |
|:--------------------------|:------------------------------------------------------------------------------|
| `min_support`             | User-defined threshold (a decimal between 0 and 1) that determines the minimum frequency at which an itemset must be present in the dataset to be considered 'frequent'.|
| `min_combination`    | User-defined minimum size of the itemsets to be considered frequent.<br><br>Setting a higher value for `min_combination` will result in the algorithm only considering larger itemsets as frequent. This can lead to discovering fewer but potentially more significant association rules or patterns. It filters out smaller itemsets, which may include common but less interesting associations.<br><br> Setting a lower value for `min_combination` allows the algorithm to find smaller frequent itemsets. This can lead to a larger number of discovered itemsets, including more specific and potentially noise patterns. It may be useful for finding fine-grained associations but can also result in a higher volume of results to analyze.|
| `max_combination`          | User-defined maximum size of the itemsets to be considered.<br><br>Setting a higher value for `max_combination` allows the algorithm to consider larger itemsets as frequent. This can be useful when you have prior knowledge that certain associations or patterns involve a larger number of items. However, it may also increase computational complexity and runtime.<br><br>Setting a lower value for `max_combination` limits the size of itemsets considered by the algorithm. It can lead to faster execution and a smaller number of results, focusing on more concise patterns. However, you might miss associations that involve larger sets of items.|

&nbsp;

**${\color{yellow}\textsf{PROS | CONS}}$**
---
| **Pros**                                          | **Cons**                                                    |
|:--------------------------------------------------|:------------------------------------------------------------|
| Efficient for finding frequent itemsets by using vertical data representation.      | Memory usage can be significant for large data.
| Suitable for datasets with high dimensionality.| Can be computationally expensive for large datasets with numerous transactions and combinations of items.|
| Does not require transaction dataset to be converted into a one-hot encoded format.| |

&nbsp;

**${\color{yellow}\textsf{IMPLEMENTATION STEPS}}$**
---
We will use the same [Market_Basket_Optimisation](https://github.com/username/repository/blob/branch/path/to/Market_Basket_Optimisation.csv) dataset to demonstrate the application of Eclat algorithm in Market Basket Analysis.

```python
# Install the pyECLAT library
pip install pyECLAT
```
```python
# Load dataset and view first 5 rows
import numpy as np
import pandas as pd
import warnings
warnings.filterwarnings("ignore")
warnings.filterwarnings("ignore", category=DeprecationWarning)
warnings.filterwarnings("ignore", category=FutureWarning, module="seaborn")
warnings.filterwarnings("ignore", category=Warning)
pd.set_option("display.max_columns", None)
pd.set_option("display.max_colwidth", None)

df = pd.read_csv("/kaggle/input/market-basket-optimisation/Market_Basket_Optimisation.csv", header=None)
df.head()
```
Below shows the first five rows of the dataset, where each row represents a basket or collection of items that are purchased together in a single transaction.<br><br>
![image](https://github.com/andytoh78/market-basket-analysis/assets/139482827/fb876432-bb1b-4907-aaa0-7b5edc3d86a9)

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
> | TID  | Item              | 
> |:---  |:----------------- | 
> | 1    | shrimp            |
> | 1    | almonds           |
> | 1    | avocado           |
> | 1    | vegetables mix    |
> | 1    | green grapes      |
> | 1    | whole weat flour  |
> | 1    | yams              |
> | 1    | cottage cheese    |
> | 1    | energy drink      |
> | 1    | tomato juice      |
> | 1    | low fat yogurt    |
> | 1    | green tea         |
> | 1    | honey             |
> | 1    | salad             |
> | 1    | mineral water     |
> | 1    | salmon            |
> | 1    | antioxydant juice |
> | 1    | frozen smoothie   |
> | 1    | spinach           |
> | 1    | olive oil         |
> | 2    | burgers           |
> | 2    | meatballs         |
> | 2    | eggs              |
> | 3    | chutney           |
> | 4    | turkey            |

Before applying the Eclat algorithm, let's first take a look at the 30 most commonly purchased items in the dataset

```python
# Find the top 30 most frequent items
top_items = df_txn['Item'].value_counts().reset_index()

# Convert the top 30 items into DataFrame and sort by item count in descending order
df_top_items = pd.DataFrame(top_items)
df_top_items.columns = ['Item', 'Count']

# Calculate the percentage of transactions for each item
total_transactions = len(df)
df_top_items['% Count'] = (df_top_items['Count']*100 / total_transactions).round(2)

# Display the results
df_top_items.style.background_gradient(cmap='Blues')
```
> | Item             | Count | % Count |
> |:-----------------|:------|:--------|
> | mineral water    | 1788  | 23.84   |
> | eggs             | 1348  | 17.97   |
> | spaghetti        | 1306  | 17.41   |
> | french fries     | 1282  | 17.09   |
> | chocolate        | 1230  | 16.40   |
> | green tea        | 991   | 13.21   |
> | milk             | 972   | 12.96   |
> | ground beef      | 737   | 9.83    |
> | frozen vegetables| 715   | 9.53    |
> | pancakes         | 713   | 9.51    |
> | burgers          | 654   | 8.72    |
> | cake             | 608   | 8.11    |
> | cookies          | 603   | 8.04    |
> | escalope         | 595   | 7.93    |
> | low fat yogurt   | 574   | 7.65    |
> | shrimp           | 536   | 7.15    |
> | tomatoes         | 513   | 6.84    |
> | olive oil        | 494   | 6.59    |
> | frozen smoothie  | 475   | 6.33    |
> | turkey           | 469   | 6.25    |
> | chicken          | 450   | 6.00    |
> | whole wheat rice | 439   | 5.85    |
> | grated cheese    | 393   | 5.24    |
> | cooking oil      | 383   | 5.11    |
> | soup             | 379   | 5.05    |
> | herb & pepper    | 371   | 4.95    |
> | honey            | 356   | 4.75    |
> | champagne        | 351   | 4.68    |
> | fresh bread      | 323   | 4.31    |
> | salmon           | 319   | 4.25    |

Mineral water is the most frequently purchased item, and it appears in 1788 (~24%) transactions. Several food items like eggs, spaghetti, french fries, chocolate, and green tea also have high purchase counts. We can also visualize the frequent items using bar charts, heatmaps, pie charts, tree maps, word cloud to better understand their distribution within the dataset.

### **${\color{lightgreen}\textsf{Bar Plot}}$**
```python
# Create the countplot to display top 30 most frequent items
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
# Create the word cloud to visualize frequent items
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
# Create the tree map to display top 30 most frequent items
import seaborn as sns
sns.set_style("whitegrid")
import squarify

# Convert 'Count' column to integers
df_top_items['Count'] = df_top_items['Count'].astype(int)

# Create a figure and axis
fig, ax = plt.subplots(figsize=(12, 6))

# Top 30 items
df_top_30items = df_top_items.head(30)

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

<br>We will now apply the Eclat algorithm to find the frequent itemsets and the association rules that are deemed as significant or interesting. 
```python
# Import the pyECLAT library
from pyECLAT import ECLAT

# Initiate an Eclat instance and load transactions DataFrame to the instance
eclat = ECLAT(data=df, verbose=True)

# Generate a binary dataframe
eclat.df_bin.head()
```
![image](https://github.com/andytoh78/market-basket-analysis/assets/139482827/df654415-d246-498a-9f93-a5c90692730d)

```python
# Display a list with all the names of the different items
unique_item_list = eclat.uniq_ 
print(unique_item_list)
```
> ##### ['barbecue sauce', 'yams', ' asparagus', 'green grapes', 'cider', 'whole weat flour', 'eggs', 'sparkling water', 'bramble', 'spinach', 'french wine', 'antioxydant juice', 'strawberries', 'butter', 'chocolate', 'chili', 'protein bar', 'tomato sauce', 'avocado', 'cooking oil', nan, 'low fat yogurt', 'champagne', 'oatmeal', 'salad', 'pasta', 'mushroom cream sauce', 'tomatoes', 'french fries', 'cereals', 'shallot', 'bug spray', 'soup', 'turkey', 'chutney', 'muffins', 'almonds', 'mint', 'soda', 'nonfat milk', 'gluten free bar', 'light cream', 'shrimp', 'grated cheese', 'rice', 'pancakes', 'carrots', 'asparagus', 'salmon', 'ketchup', 'candy bars', 'energy bar', 'escalope', 'light mayo', 'mayonnaise', 'ground beef', 'strong cheese', 'fresh bread', 'chocolate bread', 'black tea', 'babies food', 'melons', 'yogurt cake', 'brownies', 'hand protein bar', 'herb & pepper', 'red wine', 'hot dogs', 'olive oil', 'cookies', 'parmesan cheese', 'green beans', 'eggplant', 'body spray', 'cream', 'gums', 'fresh tuna', 'tea', 'white wine', 'bacon', 'whole wheat pasta', 'mashed potato', 'fromage blanc', 'shampoo', 'green tea', 'frozen vegetables', 'chicken', 'sandwich', 'salt', 'blueberries', 'pepper', 'cauliflower', 'mineral water', 'extra dark chocolate', 'meatballs', 'milk', 'whole wheat rice', 'cake', 'toothpaste', 'napkins', 'vegetables mix', 'tomato juice', 'flax seed', 'frozen smoothie', 'pickles', 'honey', 'cottage cheese', 'pet food', 'burger sauce', 'energy drink', 'magazines', 'spaghetti', 'corn', 'ham', 'zucchini', 'clothes accessories', 'mint green tea', 'dessert wine', 'water spray', 'oil', 'burgers']

```python
# Generate frequent itemsets
# Applying Eclat algorithm assuming an item has to appear in at least 4% of the total transaction to be considered as frequent and a frequent itemset should contain at least 1 item and a maximum of 3 items
min_support_threshold = 0.04
min_combination = 2
max_combination = 3

get_ECLAT_indexes, get_ECLAT_supports = eclat.fit(min_support = min_support_threshold, min_combination = min_combination, max_combination = max_combination, separator=' & ', verbose=True)

# Display results in a dataframe
result = pd.DataFrame(get_ECLAT_supports.items(),columns=['Item', 'Support'])
result = result.sort_values(by=['Support'], ascending=False).reset_index(drop=True)
result
```
> | Item                        | Support  |
> | --------------------------- | -------- |
> | mineral water & spaghetti   | 0.059725 |
> | chocolate & mineral water   | 0.05266  |
> | eggs & mineral water        | 0.050927 |
> | mineral water & milk        | 0.047994 |
> | ground beef & mineral water | 0.040928 |

## **Summary**
- The top 5 items with the highest support values are: mineral water (23.84%), eggs (17.97%), spaghetti (17.41%), french fries (17.09%), and chocolate (16.38%).<p>
- The least frequent items are fresh bread (4.31%), salmon (4.25%), and ground beef & mineral water (4.09%).<p>
- Some interesting itemsets with relatively high support include mineral water & spaghetti (5.97%), chocolate & mineral water (5.27%), and eggs & mineral water (5.09%).

