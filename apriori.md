# **Understanding the Apriori Algorithm**
---

### The Apriori algorithm,  introduced by R. Agrawal and R. Srikant in 1994, is a classic algorithm in data mining used for mining frequent itemsets and discovering association rules in a database. This page aims to provide an overview of the Apriori algorithm, its method, key parameters, advantages, limitations, and a basic implementation guide using Python.

&nbsp;

**${\color{yellow}\textsf{METHOD}}$**
---
### The Apriori algorithm operates by identifying frequent itemsets in a dataset and then extending them to larger itemsets, provided they appear sufficiently frequently in the database. It explores the itemset space in a breadth-first manner. Initially, it identifies frequent itemsets of size 1, then uses these to generate candidate itemsets of size 2, and so on, progressively increasing the size of itemsets and checking their support.

>[!Note]
>### A key principle of Apriori is that **${\color{yellow}\textsf{a subset of a frequent itemset must also be frequent}}$**. For instance, if the itemset {A, B, C} is frequent, it implies that all of its subsets, such as {A, B}, {A, C}, {B, C}, {A}, {B}, and {C}, must also be frequent. This is because any transaction containing {A, B, C} also contains all its subsets. This principle efficiently reduces the number of support calculations needed and speeds up the discovery of association rules in large datasets.

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

df = pd.read_csv("Market_Basket_Optimisation.csv", header=None)
df.head()
```
Below shows the first five rows of the dataset, where each row represents a basket or collection of items purchased in a single transaction.<br><br>
![image](https://github.com/andytoh78/market-basket-analysis/assets/139482827/edeccbd7-a940-4841-b189-2dd55f2397c9)

```python
# View shape of the dataset
print(f"The dataset contains {df.shape[0]} transactions and each transaction has a maximum of {df.shape[1]} items or less.")
```
> ##### The dataset contains 7501 transactions and each transaction has a maximum of 20 items or less.

```python
# Generate transaction lists
txns = df.fillna("").values.tolist()
txns = [[item for item in txn if item != ''] for txn in txns]
txns = [[item.strip() for item in txn] for txn in txns]

# Display first 5 lists
txns[:5]
```
> ##### [['shrimp', 'almonds', 'avocado', 'vegetables mix', 'green grapes', 'whole weat flour', 'yams', 'cottage cheese', 'energy drink', 'tomato juice', 'low fat yogurt', 'green tea', 'honey', 'salad', 'mineral water', 'salmon', 'antioxydant juice', 'frozen smoothie', 'spinach', 'olive oil'], ['burgers', 'meatballs', 'eggs'],['chutney'], ['turkey', 'avocado'], ['mineral water', 'milk', 'energy bar', 'whole wheat rice', 'green tea']]

```python
# Create a list of unique ids for the transactions
ids = [i + 1 for i in range(len(txns))]

# Initialize an empty DataFrame
data =[]
# Iterate through transactions and add them to the DataFrame with IDs
for i, txn in enumerate(txns):
    data.extend([{'TID': ids[i], 'Item': item} for item in txn])

df_txn = pd.DataFrame(data)
df_txn.head(31)
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
| 4    | avocado           |
| 5    | mineral water     |
| 5    | milk              |
| 5    | energy bar        |
| 5    | whole wheat rice  |
| 5    | green tea         |



