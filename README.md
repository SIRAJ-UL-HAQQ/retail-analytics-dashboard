# End-to-End Retail Customer Behavior & Data Analytics Pipeline

<img width="1064" height="546" alt="Screenshot 2026-07-02 213202" src="https://github.com/user-attachments/assets/500e108d-e9ae-4075-90a9-d95498e6094e" />

## Project Overview
This repository contains an end-to-end data analytics project simulating a corporate-level business intelligence workflow. The project transitions from messy, raw retail transaction data to structured database storage, advanced analytical querying, and an executive-level interactive dashboard. The pipeline uncovers the driving factors behind consumer purchasing decisions, marketing effectiveness, product demand, and subscription adoption.

## Tech Stack & Tools
* **Data Cleansing & EDA:** Python (Pandas, Jupyter Notebook)
* **Database Management:** SQL (PostgreSQL / MySQL)
* **Business Intelligence & Visualization:** Power BI

---

## Data Pipeline Architecture

### 1. Data Cleaning & Exploratory Data Analysis (Python)
* **Structural Assessment:** Handled dataset profiling, evaluated missing data, and managed structural anomalies across the transaction logs.
* **Smart Imputation:** Mitigated data bias by imputing missing product review ratings using the localized median rating within each specific product category rather than an overall global median.
* **Feature Engineering:** * Transformed categorical frequencies into numerical day-intervals (`purchase_frequency_days`) to facilitate quantitative statistical models.  
  * Segmented raw customer ages into distinct categorical demographic brackets (`age_group`: Young Adult, Middle-aged, Adult, Senior).
* **Redundancy Elimination:** Identified and dropped perfectly correlated features to optimize dataset dimensionality.

### 2. Relational Database Modeling & Advanced Analytics (SQL)
After establishing a clean dataset, the data was migrated into a relational database to run performance-critical enterprise queries to isolate deep trends:
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
Developed a fully interactive, cross-filtering dashboard tracking primary corporate KPIs across **3.9K unique customers**:
* **High-Level Metrics:** Dynamic tracking cards capturing a **$59.76 Average Purchase Amount** and a **3.75/5.00 Average Review Rating**.
* **Core Visualizations:** Cross-analyzed revenue streams versus total sales volumes across product categories and age demographics, identifying **Clothing** as the primary driver (generating over $100K in revenue).
* **Interactive Slicers:** Integrated intuitive filtering panels for Subscription Status, Gender, Category, and Shipping Type to allow stakeholders to drill down into localized data subsets instantly.

---

## Key Business Insights & Recommendations

* **Subscription Optimization Opportunity:** While repeat buyers drive consistent volume, a massive **73% of the customer base is currently unsubscribed**. The business should introduce targeted loyalty incentives or exclusive perks to transition these high-value repeat buyers into the premium subscription tier.
* **Demographic Target Prioritization:** **Young Adults** represent the highest-grossing revenue bracket and the largest sales volume segment, closely followed by **Middle-aged** consumers. Marketing campaigns and inventory planning should heavily prioritize apparel and design trends appealing to these two demographic brackets.
* **Product Category Dominance:** **Clothing** vastly outperforms all other business segments, followed by Accessories, Footwear, and Outerwear. To optimize profitability, the business should utilize bundled promotions (e.g., matching accessories with top-selling clothing items) to lift lagging categories.
* **Logistics Value Extraction:** Deep-dive analysis reveals variation in spend across delivery tiers. Standard and Express channels should be monitored to implement order minimum thresholds for expedited shipping, capturing higher margins from high-intent buyers.

---

## How to Review This Project

1. **Python Script:** Open [`/retail_customer_analysis.ipynb`](./retail_customer_analysis.ipynb) to view the data pipeline, cleaning execution, and feature engineering.
2. **SQL Queries:** Review [`/analytics_queries.sql`](./analytics_queries.sql) to inspect the analytical engine, CTEs, conditional aggregations, and window functions used to solve the core business problems.
3. **Interactive Report:** Download the [`/Executive_Sales_Dashboard.pbix`](./Executive_Sales_Dashboard.pbix) file to explore the interactive dashboard layout and cross-filtering behaviors locally in Power BI Desktop.
