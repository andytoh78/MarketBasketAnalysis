![image](https://github.com/andytoh78/MarketBasketAnalysis/assets/139482827/84b6080b-9026-43a0-9477-67f91d4ffa2f)

---
Market Basket Analysis (MBA) is a data mining technique used to discover purchasing patterns by analyzing extensive volume of transaction data. Primarily used in retail to understand customer purchase behavior, MBA helps in product placement, promotion strategies, and inventory management. The main objective is to **${\color{yellow}\textsf{detect IF-THEN patterns}}$**, find **${\color{yellow}\textsf{correlations between different items purchased together}}$**, using methods such as **${\color{yellow}\textsf{Association Rule Learning}}$**. By leveraging these techniques, retailers can extract meaningful insights from customer purchase data, leading to optimized decision-making in marketing, sales, and product management. For example : if a customer buys 'milk', then the customer is likely to buy 'bread'.


![image](https://github.com/andytoh78/MarketBasketAnalysis/assets/139482827/ab90679c-fca6-434d-9148-6a1d342d7872)

|**Process** |**Description** |
|:--- |:--- |
|**Collect Data**| The foundation of Market Basket Analysis is the collection of customer transaction data. This data typically includes key details such as the items purchased in each transaction, the time and date of purchase, and other relevant customer and transaction information. The richness and accuracy of this data are crucial, as they form the basis for all subsequent analysis and insights.|
|**Preprocess Data**| Data cleaning and preprocessing are critical steps to ensure the quality and usability of the transaction data. This phase involves removing irrelevant information that does not contribute to the analysis, such as extraneous details or erroneous entries, handling missing values, and converting/ restructuring the data into a consistent format that is compatible with the Algorithm.|
|**Use Association Rule Mining Algorithms**| Applying data mining algorithms, like Apriori or FP-Growth, to identify patterns within the data. These algorithms are designed to uncover frequent itemsets, which are groups of items that commonly appear together in transactions. These itemsets form the basis for deriving meaningful insights about customer purchasing patterns.|
|**Calculate Support and Confidence**| Two key metrics are calculated in this step: support and confidence.<br>- Support refers to the frequency of occurrence of an itemset within the dataset, indicating how common or popular a combination of items is<br>- Confidence measures the conditional probability that a specific item is purchased when another item or set of items is bought|
|**Generate Association Rules**| Based on the frequent itemsets and their corresponding support and confidence values, association rules are generated. These rules are logical statements that express the likelihood of items being purchased together. They are fundamental in understanding and leveraging the relationships unearthed in the data.|
|**Interpret Results**| Analyzing the patterns and associations identified by the Market Basket Analysis. This step is crucial for gaining insights into customer behavior and preferences. By understanding these patterns, businesses can gain a deeper understanding of how different products are interconnected in the eyes of the customers.|
|**Inform Business Decisions**| The final step is applying the insights derived from the Market Basket Analysis to inform and enhance business decisions. This can include using the findings for product recommendations, optimizing the store layout based on commonly purchased items, and developing targeted marketing campaigns. These actions can lead to increased sales, improved customer satisfaction, and more efficient store management.|

---
# **Association Rule Learning**
---
Association rule learning is a technique in data mining for discovering interesting relationships between variables in large datasets. The primary goal is to find frequent patterns, correlations, or associations from data sets found in various kinds of databases such as relational databases, transactional databases, and other forms of repositories. For instance, have you ever considered the [possibility of a connection between the purchase of diapers and beer](https://tdwi.org/articles/2016/11/15/beer-and-diapers-impossible-correlation.aspx)?

An association rule has two parts: **${\color{yellow}\textsf{antecedent (IF)}}$** and **${\color{yellow}\textsf{ consequent (THEN)}}$**. The rule suggests that if the antecedent happens, the consequent is likely to occur as well. These rules are typically used to analyze retail basket or transaction data, but are also applicable in other areas like web usage mining, intrusion detection, and bioinformatics.

<p align="center"><br>
  <img src="https://github.com/andytoh78/MarketBasketAnalysis/assets/139482827/fdabcb57-562e-45e1-ab12-1c5ad233f99a" alt="Market Basket Analysis Image">
</p>

Association rules can be classified into 3 broad categories and the strength can be measured in terms of its **${\color{yellow}\textsf{support}}$** and **${\color{yellow}\textsf{confidence}}$**.
- **${\color{yellow}\textsf{Trival}}$** (everyone knows about it!) 
- **${\color{yellow}\textsf{Inexplicable}}$** (does not make any logical sense but seems to correlated)
- **${\color{yellow}\textsf{Actionable}}$** (insights that suggest a course of action) 

---
## **Support**
---

Support (Coverage) refers to the **${\color{yellow}\textsf{frequency}}$** with which **${\color{yellow}\textsf{an itemset appears in the dataset}}$**. Ranged **${\color{yellow}\textsf{between 0 and 1}}$**, it provides an indication of how common or popular an itemset is within the given dataset, with **${\color{yellow}\textsf{0 being the least common}}$** (indicating that the itemset does not appear in any transactions) and **${\color{yellow}\textsf{1 being the most common}}$** (indicating that the itemset appears in all transactions). An itemset in the context of association rule learning is a set of one or more items that occur together in a transactional dataset.

An itemset is considered **${\color{yellow}\textsf{frequent}}$** only if the support is equal or greater than an agreed upon (user-defined) minimal value, also known as **${\color{yellow}\textsf{minimum support threshold}}$**. This principle assumes that not all item combinations are equally significant or interesting; some occur together more often than others. By focusing only the frequent itemsets, we aim to uncover the most relevant and potentially insightful patterns within the dataset.<br><br>

$$\begin{equation}
\textbf{Support}(X)=\dfrac{\textbf{Total number of transactions containing } X}{\textbf{Total number of transactions}}
\end{equation}$$


<br>For an association rule $X$ (antecedent) ⇒ $Y$ (consequent), the support is the proportion of transactions in the dataset that contain both $X$ and $Y$.<br><br>

$$\begin{equation}
\textbf{Support}(X \cup Y)=\dfrac{\textbf{Total number of transactions containing } X\textbf{ and } Y}{\textbf{Total number of transactions}}=\dfrac{(X \cup Y)}{N}
\end{equation}$$

&nbsp;

Consider the following transaction dataset (df), where each row represents a basket or transaction with a unique Transaction ID (TID), and each column represents a unique item. The values '1' and '0' indicate whether a particular item was purchased ('1') or not ('0') in each transaction. <br><br>

| TID         | Milk | Apple | Butter | Bread | Orange | Spinach |
|:----------- |:---- |:----- |:------ |:------|:------ |:------- |
| 1           | 1    | 1     | 1      | 1     | 1      | 1       |
| 2           | 0    | 1     | 0      | 1     | 1      | 1       |
| 3           | 0    | 0     | 0      | 1     | 0      | 0       |
| 4           | 1    | 1     | 0      | 0     | 1      | 1       |
| 5           | 0    | 1     | 1      | 1     | 1      | 1       |
| 6           | 1    | 0     | 1      | 1     | 1      | 0       |
| 7           | 1    | 0     | 1      | 0     | 0      | 0       |
| 8           | 1    | 1     | 0      | 1     | 1      | 0       |
| 9           | 0    | 1     | 0      | 0     | 1      | 0       |
| 10          | 1    | 1     | 1      | 1     | 1      | 0       |


<br>To calculate the **${\color{yellow}\textsf{Support}}$** (or frequency) for each item, we will need to count the number of transactions in which each item appears and then divide that count by the total number of transactions. Below is the Python code snippet to calculate the Support for each item in the transaction dataset.

```python
# Import pandas
import pandas as pd

# Create the transaction dataframe
data = {
    "TID" : [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    "Milk": [1, 0, 0, 1, 0, 1, 1, 1, 0, 1],
    "Apple": [1, 1, 0, 1, 1, 0, 0, 1, 1, 1],
    "Butter": [1, 0, 0, 0, 1, 1, 1, 0, 0, 1],
    "Bread": [1, 1, 1, 0, 1, 1, 0, 1, 0, 1],
    "Orange": [1, 1, 0, 1, 1, 1, 0, 1, 1, 1],
    "Spinach": [1, 1, 0, 1, 1, 0, 0, 0, 0, 0]
}

df = pd.DataFrame(data)

# Compute the total count of transactions in the dataset
txn_count = len(df)

# Get the list of item names, excluding TID
items = df.columns.drop('TID')

# Initialize an empty list to store support values
support_values = []

# Calculate support for each item
for itemset in items:
    support_value = df[itemset].sum() / txn_count
    support_values.append((itemset, support_value))

# Convert the results into a dataframe
df_support = pd.DataFrame(support_values, columns=['Itemset', 'Support'])

# Display the results
df_support
```

The above code snippet will return a table showing the support of each item across all transactions in the dataset. For example, Milk appears in 60% of the transactions, while Spinach appears in 40%. ​

| Item       |Support |
|:-----------|:----   |
|Milk        | 0.6    |
|Apple       | 0.7    |
|Butter      | 0.5    |
|Bread       | 0.7    |
|Orange      | 0.8    |
|Spinach     | 0.4    |
 
Finding the frequency of itemsets containing two or more items is also a common task in market basket analysis. 

```python
# Import combinations function from itertools to generate all possible combinations
from itertools import combinations

# Compute the support for each itemset of size 2
combo2_support_values_list = []
for combo in combinations(items, 2):
    combo_support = df[list(combo)].all(axis=1).mean()
    combo2_support_values_list.append((list(combo), combo_support))

# Convert the results into a dataframe
df_combo2_support = pd.DataFrame(combo2_support_values_list, columns=['Itemset', 'Support'])

# Display the results
df_combo2_support
```
The above code snippet will calculate the support values for every pair combination of items, across all the transactions in the dataset. The result is a table where each row represents a unique pair of items and their corresponding support value, indicating the frequency with which these pairs appear together in the dataset. For example, 40% of the transactions in the dataset contain both Milk and Apple.

| Itemset          | Support |
|:-----------------|:--------|
| [Milk, Apple]    | 0.4     |
| [Milk, Butter]   | 0.4     |
| [Milk, Bread]    | 0.4     |
| [Milk, Orange]   | 0.5     |
| [Milk, Spinach]  | 0.2     |
| [Apple, Butter]  | 0.3     |
| [Apple, Bread]   | 0.5     |
| [Apple, Orange]  | 0.7     |
| [Apple, Spinach] | 0.4     |
| [Butter, Bread]  | 0.4     |
| [Butter, Orange] | 0.4     |
| [Butter, Spinach]| 0.2     |
| [Bread, Orange]  | 0.6     |
| [Bread, Spinach] | 0.3     |
| [Orange, Spinach]| 0.4     |

```python
# Import combinations function from itertools to generate all possible combinations
from itertools import combinations

# Compute the support for each itemset of size 3
combo3_support_values_list = []
for combo in combinations(items, 3):
    combo_support = df[list(combo)].all(axis=1).mean()
    combo3_support_values_list.append((list(combo), combo_support))

# Convert the results into a dataframe
df_combo3_support = pd.DataFrame(combo3_support_values_list, columns=['Itemset', 'Support'])

# Display the results
df_combo3_support
```
The above code snippet is for calculating the support values for three-item combinations within the dataset.

| Itemset                    | Support |
|----------------------------|---------|
| [Milk, Apple, Butter]      | 0.2     |
| [Milk, Apple, Bread]       | 0.3     |
| [Milk, Apple, Orange]      | 0.4     |
| [Milk, Apple, Spinach]     | 0.2     |
| [Milk, Butter, Bread]      | 0.3     |
| [Milk, Butter, Orange]     | 0.3     |
| [Milk, Butter, Spinach]    | 0.1     |
| [Milk, Bread, Orange]      | 0.4     |
| [Milk, Bread, Spinach]     | 0.1     |
| [Milk, Orange, Spinach]    | 0.2     |
| [Apple, Butter, Bread]     | 0.3     |
| [Apple, Butter, Orange]    | 0.3     |
| [Apple, Butter, Spinach]   | 0.2     |
| [Apple, Bread, Orange]     | 0.5     |
| [Apple, Bread, Spinach]    | 0.3     |
| [Apple, Orange, Spinach]   | 0.4     |
| [Butter, Bread, Orange]    | 0.4     |
| [Butter, Bread, Spinach]   | 0.2     |
| [Butter, Orange, Spinach]  | 0.2     |
| [Bread, Orange, Spinach]   | 0.3     |

Combining the support for both single-item, two-item and three-item itemsets, and assuming itemsets are only considered frequent if they exist in 50% of the transactions i.e. **${\color{yellow}\textsf{minimum support threshold = 0.5}}$**, we are left with ten itemsets that meet or exceed this threshold. These frequent itemsets will be our main focus in identifying the key patterns and trends in consumer purchasing behaviour, providing valuable insights for strategic decision-making in areas such as product placement, marketing and promotions, and inventory optimization.

```python
# Combining both single-item and two-item itemsets
df_support_combined = pd.concat([df_support, df_combo2_support, df_combo3_support])

# Define minimum support threshold
min_support_threshold = 0.5

# Drop itemsets with support less than or equal to 0.5
df_support_final = df_support_combined[df_support_combined['Support'] >= min_support_threshold]

# Display the results
df_support_final
```
| Itemset          | Support |
|:-----------------|:--------|
| Milk             | 0.6     |
| Apple            | 0.7     |
| Butter           | 0.5     |
| Bread            | 0.7     |
| Orange           | 0.8     |
| ~~Spinach~~      | ~~0.4~~ |
| ~~[Milk, Apple]~~    | ~~0.4~~     |
| ~~[Milk, Butter]~~   | ~~0.4~~     |
| ~~[Milk, Bread]~~    | ~~0.4~~     |
| [Milk, Orange]   | 0.5     |
| ~~[Milk, Spinach]~~  | ~~0.2~~     |
| ~~[Apple, Butter]~~  | ~~0.3~~     |
| [Apple, Bread]   | 0.5     |
| [Apple, Orange]  | 0.7     |
| ~~[Apple, Spinach]~~ | ~~0.4~~     |
| ~~[Butter, Bread]~~  | ~~0.4~~     |
| ~~[Butter, Orange]~~ | ~~0.4~~     |
| ~~[Butter, Spinach]~~| ~~0.2~~     |
| [Bread, Orange]  | 0.6     |
| ~~[Bread, Spinach]~~ | ~~0.3~~     |
| ~~[Orange, Spinach]~~| ~~0.4~~     |
| ~~[Milk, Apple, Butter]~~      | ~~0.2~~     |
| ~~[Milk, Apple, Bread]~~       | ~~0.3~~     |
| ~~[Milk, Apple, Orange]~~      | ~~0.4~~     |
| ~~[Milk, Apple, Spinach]~~     | ~~0.2~~     |
| ~~[Milk, Butter, Bread]~~      | ~~0.3~~     |
| ~~[Milk, Butter, Orange]~~     | ~~0.3~~     |
| ~~[Milk, Butter, Spinach]~~    | ~~0.1~~     |
| ~~[Milk, Bread, Orange]~~      | ~~0.4~~     |
| ~~[Milk, Bread, Spinach]~~     | ~~0.1~~     |
| ~~[Milk, Orange, Spinach]~~    | ~~0.2~~     |
| ~~[Apple, Butter, Bread]~~     | ~~0.3~~     |
| ~~[Apple, Butter, Orange]~~    | ~~0.3~~     |
| ~~[Apple, Butter, Spinach]~~   | ~~0.2~~     |
| [Apple, Bread, Orange]     | 0.5     |
| ~~[Apple, Bread, Spinach]~~    | ~~0.3~~     |
| ~~[Apple, Orange, Spinach]~~   | ~~0.4~~     |
| ~~[Butter, Bread, Orange]~~    | ~~0.4~~     |
| ~~[Butter, Bread, Spinach]~~   | ~~0.2~~     |
| ~~[Butter, Orange, Spinach]~~  | ~~0.2~~     |
| ~~[Bread, Orange, Spinach]~~   | ~~0.3~~     |

---
## **Establish Association Rules**
---
With the frequent itemsets, we can now formulate potential association rules ($X \Rightarrow Y$). These rules are structured as if-then statements and are instrumental in evaluating the probability of relationships between different items within the dataset.

> [!NOTE]
> Since an association rule requires and is defined by an antecedent and a consequent, it can only be generated for itemsets that contain **${\color{yellow}\textsf{at least two items}}$**.

```python
# Generate association rules from two-item itemsets
association_rules = []
for index, row in df_support_final.iterrows():
    if isinstance(row['Itemset'], list) and len(row['Itemset']) == 2:
        item1, item2 = row['Itemset']
        support_item1 = df_support[df_support['Itemset'] == item1]['Support'].values[0]
        support_item2 = df_support[df_support['Itemset'] == item2]['Support'].values[0]
        association_rules.append(f'{item1} → {item2}')
        association_rules.append(f'{item2} → {item1}')

# Generate association rules from three-item itemsets
for index, row in df_support_final.iterrows():
    if isinstance(row['Itemset'], list) and len(row['Itemset']) == 3:
        item1, item2, item3 = row['Itemset']
        support_item1 = df_support[df_support['Itemset'] == item1]['Support'].values[0]
        support_item2 = df_support[df_support['Itemset'] == item2]['Support'].values[0]
        support_item3 = df_support[df_support['Itemset'] == item3]['Support'].values[0] 
        association_rules.append(f'[{item1}, {item2}] → {item3}')
        association_rules.append(f'[{item2}, {item3}] → {item1}')
        association_rules.append(f'[{item3}, {item1}] → {item2}')

# View associaion rules
association_rules
```

Below are the association rules generated from the frequent itemsets.

>['Milk → Orange',\
> 'Orange → Milk',\
> 'Apple → Bread',\
> 'Bread → Apple',\
> 'Apple → Orange',\
> 'Orange → Apple',\
> 'Bread → Orange',\
> 'Orange → Bread',\
> '[Apple, Bread] → Orange',\
> '[Bread, Orange] → Apple',\
> '[Orange, Apple] → Bread']


---
## **Confidence**
---

Confidence is a measure of how often items in $Y$ appear in transactions that contain $X$. Statistically, it is the **${\color{yellow}\textsf{conditional probability}}$** that a transaction **${\color{yellow}\textsf{containing the antecedent also contains the consequent}}$**.<br><br> 

$$\begin{equation}
\textbf{Confidence}(X \Rightarrow Y)={\textbf{P}(Y|X)}
\end{equation}$$

Mathematically, it can be expressed as the **${\color{yellow}\textsf{ratio of the support of the combined itemset}}$**(both X and Y together)  **${\color{yellow}\textsf{to the support of the antecedent itemset}}$**<br><br>

$$\begin{equation}
\textbf{Confidence}(X \Rightarrow Y)=\dfrac{\textbf{Support}(X \cup Y)}{\textbf{Support}(X)}
\end{equation}$$

&nbsp;

Confidence is a crucial metric in association rule mining as it indicates the **${\color{yellow}\textsf{strength of the relationship}}$** in an association rule. Similar to support, confidence values are also within the **${\color{yellow}\textsf{range of 0 and 1}}$**. A higher confidence value implies a stronger likelihood that customers who purchase item X will also purchase item Y, indicating a stronger association between the two items.
- Confidence of 0 :  Indicates no observed association between items X and Y. In other words, if customers buy item X, there is NO confidence that they will also buy item Y.
- Confidence of 1 : Indicates a perfect association between items X and Y. If customer buy item X, it is guaranteed that they will also buy item Y.<br><br>

Confidence values are computed specifically from the **${\color{yellow}\textsf{association rules}}$** generated **${\color{yellow}\textsf{from the frequent itemsets}}$**. Each rule ($X \Rightarrow Y$) has its own confidence value, which is calculated using the support values obtained from the data. This makes confidence a rule-specific metric, providing insights into the strength of individual rules derived from the dataset. Confidence also typically operates under a **${\color{yellow}\textsf{minimum confidence threshold}}$**. This threshold is a **${\color{yellow}\textsf{user-defined}}$** limit set to distinguish and focus only those association rules that are deemed interesting and are most likely to be meaningful and actionable.

```python
confidence_values = [] 
# Calculate confidence for two-item itemsets
for index, row in df_support_final.iterrows():
    if isinstance(row['Itemset'], list) and len(row['Itemset']) == 2:
        item1, item2 = row['Itemset']
        support_item1 = df_support[df_support['Itemset'] == item1]['Support'].values[0]
        support_item2 = df_support[df_support['Itemset'] == item2]['Support'].values[0]

        # Rule : item1 → item2
        confidence1 = row['Support'] / support_item1
        confidence_values.append({'Rule': f'{item1} → {item2}', 'Confidence': confidence1})

        # Rule : item2 → item1
        confidence2 = row['Support'] / support_item2
        confidence_values.append({'Rule': f'{item2} → {item1}', 'Confidence': confidence2})

# Calculate confidence for three-item itemsets
for index, row in df_support_final.iterrows():
    if isinstance(row['Itemset'], list) and len(row['Itemset']) == 3:
        item1, item2, item3 = row['Itemset']

        # Finding support for the combination of two items from the three-item itemset
        support_combo1 = df_support_final[df_support_final['Itemset'].apply(lambda x: set([item1, item2]).issubset(set(x)))]['Support'].values[0]
        support_combo2 = df_support_final[df_support_final['Itemset'].apply(lambda x: set([item2, item3]).issubset(set(x)))]['Support'].values[0]
        support_combo3 = df_support_final[df_support_final['Itemset'].apply(lambda x: set([item1, item3]).issubset(set(x)))]['Support'].values[0]

        # Calculate confidence for the rules
        confidence1 = row['Support'] / support_combo1
        confidence2 = row['Support'] / support_combo2
        confidence3 = row['Support'] / support_combo3

        # Append the rules
        confidence_values.append({'Rule': f'{item1}, {item2} → {item3}', 'Confidence': confidence1})
        confidence_values.append({'Rule': f'{item2}, {item3} → {item1}', 'Confidence': confidence2})
        confidence_values.append({'Rule': f'{item1}, {item3} → {item2}', 'Confidence': confidence3})

# Convert the results into a dataframe
df_confidence = pd.DataFrame(confidence_values, columns=['Rule', 'Confidence'])

# Display the results
df_confidence
```
The above code snippet will return a table showing the confidence values for the frequent itemsets. For example, when customers purchase Milk, there is a 83.3% likelihood that they will also buy Orange and when customers purchase Apple and Bread, it is guaranteed they will also buy Orange.

| Rule                  | Confidence |
|:-----------------------|:------------|
| Milk → Orange         | 0.833333   |
| Orange → Milk         | 0.625000   |
| Apple → Bread         | 0.714286   |
| Bread → Apple         | 0.714286   |
| Apple → Orange        | 1.000000   |
| Orange → Apple        | 0.875000   |
| Bread → Orange        | 0.857143   |
| Orange → Bread        | 0.750000   |
| Apple, Bread → Orange | 1.000000   |
| Bread, Orange → Apple | 0.833333   |
| Apple, Orange → Bread | 0.714286   |

---
## **Lift**
---

Lift is a measure of **${\color{yellow}\textsf{how much more often two items are bought together}}$** than what **${\color{yellow}\textsf{would be expected if their occurrences were entirely independent}}$**. It is used to assess the **${\color{yellow}\textsf{significance and strength of the association}}$** between two sets of items.

Mathematically, lift can be expressed as the ratio of the observed joint probability of both X and Y occurring together to the expected joint probability if they were independent.<br><br> 

$$\begin{equation}
\textbf{Lift}(X \Rightarrow Y)=\dfrac{\textbf{Support}(X \cup Y)}{\textbf{Support}(X) \textbf{ x } \textbf{Support}(Y)}
\end{equation}$$

- Lift > 1 : Indicates that items X and Y appear together in transactions more often than expected by chance. This suggests a positive association between the two items. In practical terms, knowing that X is present increases the likelihood of finding Y, and vice versa.
- Lift = 1: Implies that items X and Y are independent of each other, meaning their co-occurrence is as expected based on their individual frequencies.
- Lift < 1: Suggests that items X and Y appear together less often than expected by chance. This indicates a negative association or a repulsion between the two items. Knowing that X is present reduces the likelihood of finding Y, and vice versa.<br><br>

In essence, lift helps us understand whether there is a meaningful and non-random relationship between itemsets. It's a valuable metric for tasks like market basket analysis, where the objective is to identify significant item associations and make recommendations based on these associations.

```python
lift_values = []
# Calculate lift for two-item itemsets
for index, row in df_support_final.iterrows():
    if isinstance(row['Itemset'], list) and len(row['Itemset']) == 2:
        item1, item2 = row['Itemset']
        support_item1 = df_support[df_support['Itemset'] == item1]['Support'].values[0]
        support_item2 = df_support[df_support['Itemset'] == item2]['Support'].values[0]
        lift = (row['Support']) / ((support_item1)*(support_item2))
        lift_values.append({'Rule': f'[{item1}, {item2}]', 'Lift': lift})

# Calculate lift for three-item itemsets
for index, row in df_support_final.iterrows():
    if isinstance(row['Itemset'], list) and len(row['Itemset']) == 3:
        item1, item2, item3 = row['Itemset']
        support_item1 = df_support[df_support['Itemset'] == item1]['Support'].values[0]
        support_item2 = df_support[df_support['Itemset'] == item2]['Support'].values[0]        
        support_item3 = df_support[df_support['Itemset'] == item3]['Support'].values[0]        
        lift = (row['Support']) / ((support_item1)*(support_item2)*(support_item3))
        lift_values.append({'Rule': f'[{item1}, {item2}, {item3}]', 'Lift': lift})

# Convert the results into a DataFrame
df_lift = pd.DataFrame(lift_values, columns=['Rule', 'Lift'])

# Display the results
df_lift
```
| Rule                  | Lift |
|:-----------------------|:------------|
| [Milk, Orange]         | 1.041667   |
| [Apple, Bread]        | 1.020408   |
| [Apple, Orange]      | 1.250000  |
| [Bread, Orange]        | 1.071429  |
| [Apple, Bread, Orange]       | 1.275510  |

There is a slight to moderate positive association between the frequent two and three-item itemsets, indicating that the co-occurrence of these items in transactions is more frequent (lift > 1) than what would be expected by chance.

---
## **Conclusion**
---
Association Rule Learning is a powerful technique for uncovering valuable insights from transactional data, enabling the discovery of interesting patterns and relationships among items. To tackle complex datasets and real-world scenarios, there are advanced algorithms such as [Apriori](https://github.com/andytoh78/market-basket-analysis/blob/main/apriori.md), [Eclat](https://github.com/andytoh78/market-basket-analysis/blob/main/eclat.md), and [FP-growth](https://github.com/andytoh78/market-basket-analysis/blob/main/fpgrowth.md) that we can leverage on to assist in finding frequent itemsets, and uncover hidden associations, and optimize decision-making processes.
