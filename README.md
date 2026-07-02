# End-to-End Retail Customer Behavior & Data Analytics Pipeline

<img width="1064" height="546" alt="Screenshot 2026-07-02 213202" src="https://github.com/user-attachments/assets/500e108d-e9ae-4075-90a9-d95498e6094e" />


## Project Overview
This repository contains an end-to-end data analytics project simulating a corporate-level business intelligence workflow. The project transitions from messy, raw retail transaction data to structured database storage, advanced analytical querying, and an executive-level interactive dashboard to uncover driving factors behind consumer purchasing decisions, marketing effectiveness, and brand loyalty.

## Tech Stack & Tools
* **Data Cleansing & EDA:** Python (Pandas, Jupyter Notebook)
* **Database Management:** SQL (PostgreSQL / MySQL)
* **Business Intelligence & Visualization:** Power BI, Microsoft Excel

---

## Data Pipeline Architecture

### 1. Data Cleaning & Exploratory Data Analysis (Python)
* **Structural Assessment:** Handled dataset profiling, evaluated missing data, and managed structural anomalies.
* **Smart Imputation:** Mitigated data bias by imputing missing product review ratings using the localized median rating within each specific product category rather than an overall global median.
* **Feature Engineering:**   
  * Transformed categorical frequencies into numerical day-intervals (`purchase_frequency_days`) to facilitate quantitative statistical models.  
  * Segmented raw customer ages into distinct categorical demographic brackets (`age_group`).
* **Redundancy Elimination:** Identified and dropped perfectly correlated features to optimize dataset dimensionality.

### 2. Relational Database Modeling & Advanced Analytics (SQL)
After establishing a clean dataset, the data was migrated into a relational database to run performance-critical enterprise queries:
* **Demographic Revenue Splits:** Aggregated total revenue performance across gender and age distributions.
* **Customer Segmentation:** Implemented Common Table Expressions (CTEs) to segment the customer base into `New`, `Returning`, and `Loyal` cohorts based on historical transaction volumes.
* **Window Functions for Market Trends:** Utilized `ROW_NUMBER()` window functions to isolate and rank the top 3 best-selling products within every individual retail category.
* **Promotion & Shipping Analysis:** Modeled the exact operational impact of discount rates on individual items and isolated average order values across standard vs. express shipping channels.

> **SQL Highlight: Category-Specific Product Ranking Using Window Functions**
> ```sql
> WITH item_counts AS (
>     SELECT category,
>            item_purchased,
>            COUNT(customer_id) AS total_orders,
>            ROW_NUMBER() OVER (PARTITION BY category ORDER BY COUNT(customer_id) DESC) AS item_rank
>     FROM customer
>     GROUP BY category, item_purchased
> )
> SELECT category, item_purchased, total_orders, item_rank
> FROM item_counts
> WHERE item_rank <= 3;
> ```

### 3. Executive Business Intelligence Dashboard (Power BI)
Developed a fully interactive, cross-filtering dashboard focused on primary corporate KPIs:
* **High-Level Metrics:** Dynamic tracking cards for Total Customer Count, Average Purchase Value, and Average Review Ratings.
* **Core Visualizations:** Cross-analyzed revenue streams versus total sales volumes across product categories and age demographics.
* **Interactive Slicers:** Integrated intuitive filtering panels for Subscription Status, Gender, Category, and Shipping Type to allow stakeholders to drill down into localized data subsets instantly.

---

## Key Business Insights & Recommendations
* **Subscription Optimization:** A substantial portion of high-volume, repeat buyers remain unsubscribed. The business should introduce targeted loyalty incentives to transition these high-value repeat buyers into the subscription model.
* **Demographic Target:** Young adults represent the highest-grossing revenue bracket. Marketing campaigns and product lines should heavily prioritize trends appealing to this segment.
* **Logistics Value:** Express shipping consistently yields higher average purchase amounts compared to standard shipping, suggesting that promotional bundling with expedited delivery can optimize average order values.

---

## How to Review This Project
1. **Python Script:** Open [`/retail_customer_analysis.ipynb`](./retail_customer_analysis.ipynb) to view the data pipeline, cleaning execution, and feature engineering.
2. **SQL Queries:** Review [`/analytics_queries.sql`](./analytics_queries.sql) to inspect the analytical engine, CTEs, and window functions used to solve the core business problem statements.
3. **Interactive Report:** Download the [`/Executive_Sales_Dashboard.pbix`](./Executive_Sales_Dashboard.pbix) file to explore the interactive dashboard layout locally in Power BI Desktop.
