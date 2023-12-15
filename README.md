![image](https://github.com/andytoh78/MarketBasketAnalysis/assets/139482827/84b6080b-9026-43a0-9477-67f91d4ffa2f)

---
### Market Basket Analysis (MBA) is a data mining technique used to discover purchasing patterns by analyzing extensive volume of transaction data. Primarily used in retail to understand customer purchase behavior, MBA helps in product placement, promotion strategies, and inventory management. The main objective is to detect "if-then" patterns, find correlations between different items purchased together, using methods such as Association Rule Learning. By leveraging these techniques, retailers can extract meaningful insights from customer purchase data, leading to optimized decision-making in marketing, sales, and product management. For example : if a customer buys 'milk', then the customer is likely to buy 'bread'.
---

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
### Association rule learning is a technique in data mining for discovering interesting relationships between variables in large datasets. The primary goal is to find frequent patterns, correlations, or associations from data sets found in various kinds of databases such as relational databases, transactional databases, and other forms of repositories. For instance, have you ever considered the [possibility of a connection between the purchase of diapers and beer](https://tdwi.org/articles/2016/11/15/beer-and-diapers-impossible-correlation.aspx)?

### An association rule has two parts: **${\color{yellow}\texttt{antecedent (IF)}}$** and **${\color{yellow}\texttt{ consequent (THEN)}}$**. The rule suggests that if the antecedent happens, the consequent is likely to occur as well. These rules are typically used to analyze retail basket or transaction data, but are also applicable in other areas like web usage mining, intrusion detection, and bioinformatics.

<p align="center"><br>
  <img src="https://github.com/andytoh78/MarketBasketAnalysis/assets/139482827/fdabcb57-562e-45e1-ab12-1c5ad233f99a" alt="Market Basket Analysis Image">
</p>

### Association rules can be classified into 3 broad categories and the strength can be measured in terms of its **${\color{yellow}\texttt{support}}$** and **${\color{yellow}\texttt{confidence}}$**.
- ### **${\color{yellow}\texttt{Trival}}$** (everyone knows about it!) 
- ### **${\color{yellow}\texttt{Inexplicable}}$** (does not make any logical sense but seems to correlated)
- ### **${\color{yellow}\texttt{Actionable}}$** (insights that suggest a course of action) 

---
## **Support**
---

### Support (Coverage) refers to the **${\color{yellow}\texttt{frequency}}$** with which **${\color{yellow}\texttt{an itemset appears in the dataset}}$**. It provides an indication of how common or popular an itemset is within the given dataset. An itemset in the context of association rule learning is a set of one or more items that occur together in a transactional dataset.

### An itemset is considered **${\color{yellow}\texttt{frequent}}$** only if the support is equal or greater than an agreed upon minimal value, also known as **${\color{yellow}\texttt{minimum support threshold}}$**.<br><br>

$$\begin{equation}
\textbf{Support}(X)=\dfrac{\textbf{Total number of transactions containing } X}{\textbf{Total number of transactions}}
\end{equation}$$


### <br>For an association rule $X$ (antecedent) ⇒ $Y$ (consequent), the support is the proportion of transactions in the dataset that contain both $X$ and $Y$.<br><br>

$$\begin{equation}
\textbf{Support}(X \cup Y)=\dfrac{\textbf{Total number of transactions containing } X\textbf{ and } Y}{\textbf{Total number of transactions}}=\dfrac{(X \cup Y)}{N}
\end{equation}$$

&nbsp;

### Consider the following transaction dataset (df), where each row represents a basket or transaction with a unique Transaction ID (TID), and each column represents a unique item. The values '1' and '0' indicate whether a particular item was purchased ('1') or not ('0') in each transaction. <br><br>

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


### <br>To calculate the **${\color{yellow}\texttt{Support}}$** (or frequency) for each item, we will need to count the number of transactions in which each item appears and then divide that count by the total number of transactions. Below is the Python code snippet to calculate the Support for each item in the transaction dataset.

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

# Initialize an empty dictionary to store support values
support_values = {}

# Calculate support for each item
for item in items:
    support_values[item] = df[item].sum()/txn_count

# Display the suppory values for all the items in the transaction dataset
support_values
```

### The above 
