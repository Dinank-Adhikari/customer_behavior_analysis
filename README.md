# customer_behavior_analysis

An end-to-end data analytics project that explores 3,900 retail transactions to understand spending patterns, customer segments, product preferences, and subscription behavior. The workflow covers data cleaning in Python, business analysis in SQL Server, an interactive Power BI dashboard, a written report, and a presentation built with Gamma.

---

## Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Tools & Technologies](#tools--technologies)
- [Project Steps](#project-steps)
- [Dashboard](#dashboard)
- [Key Results](#key-results)
- [Business Recommendations](#business-recommendations)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Author](#author)

---

## Overview

**Goal:** Turn raw transactional data into insights that can guide marketing, pricing, and loyalty decisions.

**Business questions answered:**

- Who spends more, and which customer groups drive revenue?
- Do subscribers spend more than non-subscribers?
- Which products are top-rated, most purchased, or most dependent on discounts?
- How are customers split between New, Returning, and Loyal?
- Which age groups contribute the most revenue?

---

## Dataset

| Item | Details |
|------|---------|
| File | `customer_shopping_behavior.csv` |
| Rows | 3,900 |
| Columns | 18 |
| Missing values | 37 in `Review Rating` |

**Main feature groups**

- **Customer demographics:** Age, Gender, Location, Subscription Status
- **Purchase details:** Item Purchased, Category, Purchase Amount (USD), Season, Size, Color
- **Shopping behavior:** Discount Applied, Promo Code Used, Previous Purchases, Frequency of Purchases, Review Rating, Shipping Type, Payment Method

**Engineered features**

- `age_group`: customers split into four quartile-based groups (Young Adult, Adult, Middle-aged, Senior)
- `purchase_frequency_days`: purchase frequency text converted to number of days

---

## Tools & Technologies

| Area | Tools |
|------|-------|
| Data cleaning & EDA | Python, pandas, Jupyter Notebook |
| Database | Microsoft SQL Server, SQL Server Management Studio (SSMS) |
| Python-to-SQL connection | SQLAlchemy, pyodbc |
| Visualization | Power BI |
| Reporting | PDF report |
| Presentation | Gamma |

---

## Project Steps

### 1. Data Loading and EDA (Python)
- Loaded the CSV with pandas
- Reviewed structure with `df.info()` and summary statistics with `df.describe()`
- Checked for null values

### 2. Data Cleaning (Python)
- Filled missing `Review Rating` values using the **median rating of each product category**
- Standardized column names to `snake_case`
- Created the `age_group` and `purchase_frequency_days` columns
- Confirmed `discount_applied` and `promo_code_used` were identical, then dropped `promo_code_used`

### 3. Database Integration
- Connected Python to SQL Server using SQLAlchemy and pyodbc
- Loaded the cleaned DataFrame into the `customer_behavior` database as `dbo.customer`

### 4. SQL Analysis (SQL Server)
Ten business questions were answered using aggregations, subqueries, CTEs, `CASE` statements, and window functions (`ROW_NUMBER`). All queries are in `customer_behavior_sql_queries.sql`.

| # | Business Question |
|---|-------------------|
| 1 | Revenue by gender |
| 2 | Discount users who spent above the average purchase amount |
| 3 | Top 5 products by average review rating |
| 4 | Average purchase amount: Standard vs Express shipping |
| 5 | Subscribers vs non-subscribers (spend and revenue) |
| 6 | Top 5 products by share of discounted purchases |
| 7 | Customer segmentation: New, Returning, Loyal |
| 8 | Top 3 most purchased products in each category |
| 9 | Repeat buyers (more than 5 purchases) and subscription status |
| 10 | Revenue contribution by age group |

### 5. Dashboard (Power BI)
Built an interactive dashboard with slicers and KPI cards to explore the findings visually.

### 6. Report and Presentation
- Wrote a project report summarizing the process, results, and recommendations
- Created a presentation using Gamma

---

## Dashboard

The Power BI dashboard (`Customer_Behavior_Dashboard.pbix`) includes:

- **KPI cards:** Number of Customers (3.9K), Average Purchase Amount ($59.76), Average Review Rating (3.75)
- **Charts:** Subscription status split, Revenue by Category, Sales by Category, Revenue by Age Group, Sales by Age Group
- **Slicers:** Subscription Status, Gender, Category, Shipping Type


---

## Key Results

| Insight | Finding |
|---------|---------|
| Revenue by gender | Male customers: $157,890, Female customers: $75,191 |
| Subscription | 27% of customers are subscribers; average spend is nearly the same (about $59.49 vs $59.87) |
| Shipping | Express shipping has a slightly higher average purchase ($60.48 vs $58.46 for Standard) |
| Customer segments | Loyal: 3,116, Returning: 701, New: 83 |
| Age groups | Young Adult brings the highest revenue ($62,143), followed by Middle-aged ($59,197) |
| Categories | Clothing generates the most revenue, followed by Accessories |
| Discounts | Hat, Sneakers, Coat, Sweater, and Pants have the highest discount rates (about 47% to 50%) |
| High-spending discount users | 863 customers used a discount but still spent above the average |
| Repeat buyers | Among customers with more than 5 previous purchases, 958 are subscribed and 2,518 are not |

---

## Business Recommendations

- **Boost subscriptions:** Promote exclusive benefits, since subscribers currently make up only a small share of customers.
- **Strengthen loyalty programs:** Reward repeat buyers to keep them in the Loyal segment and move Returning customers up.
- **Review discount policy:** Balance sales growth with margin, especially for products that rely heavily on discounts.
- **Improve product positioning:** Highlight top-rated and best-selling products in campaigns.
- **Target marketing:** Focus on high-revenue age groups and express-shipping users.

---

## Project Structure

```
Customer_Behavior_Analysis/
│
├── customer_shopping_behavior.csv            # Raw dataset
├── Customer_Shopping_Behavior_Analysis.ipynb # Python cleaning and EDA
├── customer_behavior_sql_queries.sql         # SQL Server queries (10 questions)
├── Customer_Behavior_Dashboard.pbix          # Power BI dashboard
├── Customer_Shopping_Behavior_Analysis.pdf   # Project report
└── Customer-Shopping-Behavior-Analysis.pptx  # Presentation (created with Gamma)
```

---

## How to Run

### Prerequisites
- Python 3.9 or higher
- Microsoft SQL Server (Express edition works) and SSMS
- [ODBC Driver 17 for SQL Server](https://learn.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server)
- Power BI Desktop

### Steps

**1. Clone the repository**
```bash
https://github.com/Dinank-Adhikari/customer_behavior_analysis.git
cd customer_behavior_analysis.

**2. Install Python libraries**
```bash
pip install pandas sqlalchemy pyodbc jupyter
```

**3. Create the database**

In SSMS, run:
```sql
CREATE DATABASE customer_behavior;
```

**4. Run the notebook**

Open `Customer_Shopping_Behavior_Analysis.ipynb` in Jupyter and update the SQL Server connection string to match your machine:

```python
"DRIVER={ODBC Driver 17 for SQL Server};"
"SERVER=YOUR_SERVER_NAME\\SQLEXPRESS;"
"DATABASE=customer_behavior;"
"Trusted_Connection=yes;"
"TrustServerCertificate=yes;"
```

Run all cells. This cleans the data and creates the `dbo.customer` table.

**5. Run the SQL queries**

Open `customer_behavior_sql_queries.sql` in SSMS and run the queries one by one.

**6. Open the dashboard**

Open `Customer_Behavior_Dashboard.pbix` in Power BI Desktop. If the data does not load, go to **Transform data > Data source settings** and point it to your SQL Server instance.

---

## Author

Dinank Adhikari
B.Tech in Computer Science and Engineering
[LinkedIn](https://www.linkedin.com/in/dinank-adhikari-3138a7229/). | [GitHub](https://github.com/Dinank-Adhikari) | adhakri86@gmail.com
