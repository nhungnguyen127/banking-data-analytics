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




