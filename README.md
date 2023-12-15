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
