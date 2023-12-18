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
```python
# Import required libraries and load the [Market_Basket_Optimisation](https://github.com/username/repository/blob/branch/path/to/Market_Basket_Optimisation.csv)
 datatset.

```












