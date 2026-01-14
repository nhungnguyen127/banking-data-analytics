# Banking Transaction & Customer Analytics

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#project-overview)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Findings & Recommendations](#findings-&-recommendations)

### Project Overview
This project aims to provide insights into **banking transaction performance** by analyzing customer transactions, card usage, and fraud-related activities.  
By leveraging **SQL Server** for data processing, the project identifies spending patterns, key risk indicators, and actionable insights to support data-driven decision making in the banking domain.

### Data Sources
The project uses a publicly available synthetic banking dataset designed for analytical and educational purposes, including transaction records, customer information, and card details.

### Tools
- Excel - Data Cleaning
- SQL Server - Data Analysis
  
### Data Cleaning 
In the initial data preparation phase, I performed the following tasks:
**1. Data loading and structure inspection**  
   - Imported raw CSV files into SQL Server tables  
   - Reviewed column data types and identified incorrect automatic type inference  

**2. Data type standardization**
   - Converted transaction amounts, balances, and credit limits to DECIMAL(18,2)  
   - Converted large identifier fields to BIGINT to prevent arithmetic overflow errors  
   - Standardized date fields to DATE and DATETIME formats  

**3. Data integrity and relationship validation**  
   - Ensured consistency between related tables (users, cards, transactions)  
   - Verified primary and foreign key relationships
  
### Exploratory Data Analysis
**1. Transaction Data**
- The transaction dataset contains 11.28 million transactions with a total transaction value of $485.3 million. The average transaction amount is approximately $43, indicating that the majority of transactions are relatively low-value and reflect everyday consumer spending behavior.

- Transaction amounts range from - $500.00 to $6,820.20. Most transactions fall within the $0–$100 range, with the $10–$50 bucket being the most frequent. However, higher-value transactions between $50–$500 contribute disproportionately to total transaction value. The presence of negative transaction values suggests refunds or reversals, which are common in real-world banking transaction systems and should be treated as a distinct transaction category in further analysis.
  
- Swipe transactions account for the majority of transaction volume, while online transactions represent a smaller but notable share. Transaction activity peaks between 10 AM and 3 PM, before gradually declining in the evening. Slightly higher transaction activity is observed in January and March, while mid-year months show somewhat lower volumes.

**2. Customers Data**
- The customer dataset includes 2,000 unique customers with an average age of 45, spanning from 18 to 101 years old, indicating a broad and balanced age distribution across customer segments. Middle-aged groups account for a large proportion of customers, reflecting a financially active population.

- The average yearly income is approximately $45,716, while the average total debt reaches $63,710, suggesting that many customers carry significant financial obligations relative to income. Despite this, overall credit quality remains strong, with an average credit score of 709.

- Most customers fall into the Good and Excellent credit score categories, while only a small fraction are classified as Fair or Poor. Credit scores remain relatively stable across income groups, indicating consistent credit behavior regardless of income level.

**3. Cards Data**
- The cards dataset includes 6,146 cards issued to 2,000 customers.
  
- The average credit limit is $14,347, ranging from $0 to $151,223, indicating significant variation in customer credit capacity.
  
- Debit cards account for the majority of issued cards, while credit cards represent a smaller but important segment. Visa and Mastercard dominate across both debit and credit card types, with premium brands such as American Express and Discover serving higher-end customer segments.
  
### Data Analysis
**1. Customer Spending Behavior & Revenue Drivers**
```sql
  SELECT
    CASE
        WHEN credit_score < 580 THEN 'Poor'
        WHEN credit_score BETWEEN 580 AND 669 THEN 'Fair'
        WHEN credit_score BETWEEN 670 AND 739 THEN 'Good'
        ELSE 'Excellent'
    END AS credit_group,
    COUNT(DISTINCT u.id) AS num_customers,
    SUM(t.amount) AS total_spent,
    AVG(t.amount) AS avg_spent_per_customer
FROM Users_data u
JOIN Transactions_data t
    ON u.id = t.client_id
GROUP BY  CASE
        WHEN credit_score < 580 THEN 'Poor'
        WHEN credit_score BETWEEN 580 AND 669 THEN 'Fair'
        WHEN credit_score BETWEEN 670 AND 739 THEN 'Good'
        ELSE 'Excellent'
    END
ORDER BY total_spent DESC;
```
**2. Credit Score & Credit Utilization**
```sql
SELECT
    CASE 
        WHEN yearly_income < 30000 THEN 'Low income'
        WHEN yearly_income BETWEEN 30000 AND 70000 THEN 'Middle income'
        ELSE 'High income'
    END AS income_group,
    COUNT(DISTINCT t.client_id) AS customers,
    SUM(t.amount) AS total_spent,
    AVG(t.amount) AS avg_transaction_value
FROM Users_data u
JOIN Transactions_data t 
    ON u.id = t.client_id
GROUP BY CASE 
        WHEN yearly_income < 30000 THEN 'Low income'
        WHEN yearly_income BETWEEN 30000 AND 70000 THEN 'Middle income'
        ELSE 'High income'
    END
```
**3. Income Level & Card Ownership**
```sql
WITH card_utilization AS (
    SELECT
        c.id AS card_id,
        c.client_id,
        SUM(t.amount) AS total_spending,
        c.credit_limit,
        SUM(t.amount) / NULLIF(c.credit_limit, 0) AS utilization_ratio
    FROM Cards_data c
    JOIN Transactions_data t
        ON c.id = t.card_id
    GROUP BY c.id, c.client_id, c.credit_limit
)
SELECT
    CASE
        WHEN utilization_ratio < 0.3 THEN 'Low Utilization (<30%)'
        WHEN utilization_ratio BETWEEN 0.3 AND 0.7 THEN 'Medium Utilization (30–70%)'
        ELSE 'High Utilization (>70%)'
    END AS utilization_group,
    COUNT(DISTINCT client_id) AS total_customers,
    COUNT(card_id) AS total_cards,
    ROUND(AVG(utilization_ratio) * 100, 2) AS avg_utilization_pct,
    ROUND(
        COUNT(DISTINCT client_id) * 100.0 /
        SUM(COUNT(DISTINCT client_id)) OVER (),
        2
    ) AS customer_share_pct
FROM card_utilization
GROUP BY
    CASE
        WHEN utilization_ratio < 0.3 THEN 'Low Utilization (<30%)'
        WHEN utilization_ratio BETWEEN 0.3 AND 0.7 THEN 'Medium Utilization (30–70%)'
        ELSE 'High Utilization (>70%)'
    END
ORDER BY customer_share_pct DESC;
```
**4. Payment Method & Transaction Risk**
```sql
SELECT 
use_chip,
total_transactions,
error_transactions,
ROUND((error_transactions * 1.0 /total_transactions)*100,4) AS error_rate
FROM (SELECT
    use_chip,
    COUNT(*) AS total_transactions,
    SUM(CASE WHEN errors <> '' THEN 1 ELSE 0 END) AS error_transactions
FROM Transactions_data
GROUP BY use_chip) t
```
**5. Spending Categories & Merchant Strategy**
```sql
SELECT
  m.mcc,
  services,
  SUM(t.amount) AS total_spent
FROM transactions_data t
LEFT JOIN merchants_data m ON m.mcc = t.mcc
GROUP BY m.mcc, services
ORDER BY total_spent DESC;
```

### Findings & Recommendations
**1. Customer Spending Behavior & Revenue Drivers**

**Findings**
- Total transaction revenue is driven by both transaction frequency and average transaction value, not frequency alone.
- Two distinct high-revenue segments exist:
  - High-frequency, low-value customers
  - Low-frequency, high-value customers

**Recommendations**
- Segment high-revenue customers by usage pattern:
- Retain high-frequency users via loyalty and usage-based incentives.
- Target high-value users with premium cards, higher limits, and stricter risk controls.

**2. Credit Score & Credit Utilization**

**Findings**
- Customers with Good and Excellent credit scores contribute most to total transaction revenue due to population size.
- Average spending per customer is similar across credit score groups, suggesting scale matters more than intensity.
- A majority of customers fall into the high credit utilization group (>70%), indicating heavy reliance on credit limits.

**Recommendations**
- Prioritize Good & Excellent credit score customers for profitable products and cross-selling.
- Closely monitor high-utilization customers and apply gradual limit adjustments or tailored repayment programs to balance growth and risk.

**3. Income Level & Card Ownership**

**Findings**
- Middle-income customers generate the highest total transaction value due to scale.
- High-income customers show significantly higher average transaction values.
- Customers holding 3–4 cards generate the highest total spending; spending declines beyond this point.

 **Recommendations**
- Apply volume-based strategies for middle-income segments.
- Offer premium, high-value products to high-income customers.
- Encourage customers to hold 3–4 cards, avoiding diminishing returns from excessive card issuance.

**4. Payment Method & Transaction Risk**

**Findings**
- Credit cards have the highest average transaction value.
- Debit cards drive the largest transaction volume and total spending.
- Prepaid debit cards show low spending efficiency.
- Online transactions have the highest error rate, indicating higher operational and fraud risk.

**Recommendations**
- Prioritize credit cards for profitability and high-value spending.
- Continue leveraging debit cards for volume.
- Limit prepaid debit cards to basic or entry-level use cases.
- Strengthen monitoring and fraud controls for online transactions.

**5. Spending Categories & Merchant Strategy**

**Findings**
- Spending is concentrated in essential and recurring categories (money transfers, groceries, utilities, fuel).
- Discretionary categories contribute significantly less to total spending.
  
**Recommendations**
- Prioritize partnerships, promotions, and rewards in essential categories to maximize volume.
- Selectively target discretionary categories for upselling and lifestyle-based campaigns.





