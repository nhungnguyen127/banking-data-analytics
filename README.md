# Banking Transaction & Customer Analytics

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#project-overview)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Findings](#findings)
- [Recommendations](#recommendations)


### Project Overview
This project aims to provide insights into **banking transaction performance** by analyzing customer transactions, card usage, and fraud-related activities.  
By leveraging **SQL Server** for data processing and **Power BI** for visualization, the project identifies spending patterns, key risk indicators, and actionable insights to support data-driven decision making in the banking domain.

### Data Sources
The project uses a publicly available synthetic banking dataset designed for analytical and educational purposes, including transaction records, customer information, and card details.

### Tools
- Excel - Data Cleaning
- SQL Server - Data Analysis
- Power BI - Creating reports
  
### Data Cleaning 
In the initial data preparation phase, I performed the following tasks:
1. Data loading and structure inspection  
   - Imported raw CSV files into SQL Server tables  
   - Reviewed column data types and identified incorrect automatic type inference  

2. Data type standardization  
   - Converted transaction amounts, balances, and credit limits to DECIMAL(18,2)  
   - Converted large identifier fields to BIGINT to prevent arithmetic overflow errors  
   - Standardized date fields to DATE and DATETIME formats  

3. Data integrity and relationship validation  
   - Ensured consistency between related tables (users, cards, transactions)  
   - Verified primary and foreign key relationships
  
### Exploratory Data Analysis
1. Transaction Data
- The transaction dataset contains 11.28 million transactions with a total transaction value of $485.3 million. The average transaction amount is approximately $43, indicating that the majority of transactions are relatively low-value and reflect everyday consumer spending behavior.

- Transaction amounts range from - $500.00 to $6,820.20. Most transactions fall within the $0–$100 range, with the $10–$50 bucket being the most frequent. However, higher-value transactions between $50–$500 contribute disproportionately to total transaction value. The presence of negative transaction values suggests refunds or reversals, which are common in real-world banking transaction systems and should be treated as a distinct transaction category in further analysis.
  
- Swipe transactions account for the majority of transaction volume, while online transactions represent a smaller but notable share. Transaction activity peaks between 10 AM and 3 PM, before gradually declining in the evening. Slightly higher transaction activity is observed in January and March, while mid-year months show somewhat lower volumes.

2. Customers Data
- The customer dataset includes 2,000 unique customers with an average age of 45, spanning from 18 to 101 years old, indicating a broad and balanced age distribution across customer segments. Middle-aged groups account for a large proportion of customers, reflecting a financially active population.

- The average yearly income is approximately $45,716, while the average total debt reaches $63,710, suggesting that many customers carry significant financial obligations relative to income. Despite this, overall credit quality remains strong, with an average credit score of 709.

- Most customers fall into the Good and Excellent credit score categories, while only a small fraction are classified as Fair or Poor. Credit scores remain relatively stable across income groups, indicating consistent credit behavior regardless of income level.

3. Cards Data
- The cards dataset includes 6,146 cards issued to 2,000 customers.
  
- The average credit limit is $14,347, ranging from $0 to $151,223, indicating significant variation in customer credit capacity.
  
- Debit cards account for the majority of issued cards, while credit cards represent a smaller but important segment. Visa and Mastercard dominate across both debit and credit card types, with premium brands such as American Express and Discover serving higher-end customer segments.
  
### Data Analysis
```sql
  SELECT * FROM
```

### Findings
The analysis results are sumarized as follows:
1.
2.
3.

### Recommendations
Based on the analysis, I recommend the following actions:

 - 
### Limitations

### References


😄
|Heading1|Heading2|
|---------|--------|
|Content|Content2|
|Python|SQL|

`column_1`
**bold**
*italic*




