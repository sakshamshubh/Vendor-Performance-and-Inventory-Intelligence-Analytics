# Vendor Performance and Inventory Intelligence Analytics

An end-to-end data analytics project that evaluates vendor performance, pricing strategy, and inventory efficiency for a retail/wholesale business, using SQL-based data engineering, Python-based exploratory analysis, and a Power BI dashboard for stakeholder-facing insights.

---

## Business Problem

Effective inventory and sales management are critical for optimizing profitability in the retail and wholesale industry. Companies need to ensure they are not incurring losses due to inefficient pricing, poor inventory turnover, or vendor dependency.

**Goals of this analysis:**
- Identify underperforming brands that require promotional or pricing adjustments
- Determine top vendors contributing to sales and gross profit
- Analyze the impact of bulk purchasing on unit costs
- Assess inventory turnover to reduce holding costs and improve efficiency
- Provide actionable recommendations for pricing, vendor management, and inventory strategy

---

## Tech Stack

| Category | Tools |
|---|---|
| Language | Python, SQL |
| Database | SQLite (via SQLAlchemy) |
| Data Handling | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| Statistical Analysis | Scipy |
| BI & Dashboarding | Power BI |
| Logging | Python `logging` module |

---

## Project Workflow

**1. Data Ingestion**
- Six raw source tables (`begin_inventory`, `end_inventory`, `purchases`, `purchase_prices`, `sales`, `vendor_invoice`) ingested into a local SQLite database
- Built a reusable ingestion pipeline with execution logging and runtime tracking

**2. Exploratory Data Analysis**
- Explored all six raw tables to understand structure, relationships, and relevance to the business problem
- Identified that `begin_inventory` and `end_inventory` lacked vendor-level detail and were excluded from further analysis
- Designed and validated a consolidated SQL query joining purchases, sales, and freight data at the vendor-brand level

**3. Vendor Sales Summary Table**
- Built an optimized SQL query using CTEs to merge purchase, sales, and freight summaries by vendor and brand
- Engineered key business metrics:
  - **Gross Profit** = Total Sales − Total Purchase
  - **Profit Margin** = Gross Profit / Total Sales
  - **Stock Turnover** = Sales Quantity / Purchase Quantity
  - **Sales-to-Purchase Ratio**
- Cleaned and standardized the resulting table, and persisted it back into the database for downstream analysis

**4. Data Analysis**
- Analyzed summary statistics to identify data quality issues, including transactions with negative gross profit and unsold inventory
- Examined correlations between pricing, sales, and profitability (e.g. purchase price showed weak correlation with sales revenue)
- Identified brands with **low sales but high profit margins** — strong candidates for pricing or promotional adjustments
- Quantified vendor concentration risk by measuring what share of total procurement comes from top vendors
- Analyzed bulk purchasing impact: large order sizes reduced unit cost by ~72% compared to small orders
- Identified vendors with low inventory turnover and quantified capital locked in unsold stock per vendor

**5. Dashboard**
- Built an interactive Power BI dashboard summarizing total sales, purchases, gross profit, profit margin, unsold capital, top vendors/brands by sales, and low-performing vendors and brands

---

## Key Findings

- **Total Sales:** $441.41M | **Total Purchase:** $307.34M | **Gross Profit:** $134.07M | **Profit Margin:** 38.72%
- **Unsold Capital:** $2.71M locked in inventory that has not converted to sales
- **Diageo North America** is the top vendor by sales (~$68M), followed by Martignetti Companies and Pernod Ricard USA
- **Jack Daniels, Tito's Handmade Vodka,** and **Grey Goose** are the top-selling brands
- Bulk purchasing significantly reduces unit cost — vendors ordering in large volumes pay as little as $10.78 per unit
- A small group of vendors accounts for a large share of total procurement, indicating meaningful vendor concentration risk

---

## Final Recommendations

- Re-evaluate pricing for low-sales, high-margin brands to boost sales volume without sacrificing profitability
- Diversify vendor partnerships to reduce dependency on a few suppliers and mitigate supply chain risk
- Leverage bulk purchasing advantages to maintain competitive pricing while optimizing inventory management
- Optimize slow-moving inventory through adjusted purchase quantities, clearance strategies, or revised storage
- Enhance marketing and distribution support for low-performing vendors to drive sales without compromising margins

---

## Repository Structure

```
├── notebooks/
│   ├── 1 Ingestion of data into database.ipynb   # Loads raw CSVs into SQLite
│   ├── 2 Exploratory Data Analysis.ipynb          # Table-level exploration & query design
│   ├── 3 Vendor Performance Analysis.ipynb        # Core analysis & business insights
│   ├── Business Problem.ipynb                     # Problem statement & goals
│   └── exportProcessedData.ipynb                  # Exports final summary table to CSV
├── scripts/
│   ├── ingestion_db_1.py                          # Reusable data ingestion script
│   └── get_vendor_summary_2.py                    # Builds & cleans the vendor summary table
├── data/
│   └── vendor_sales_summary.csv                   # Final processed dataset (raw source data not included due to size)
├── dashboard/
│   └── Vendor Analytics PowerBI Dashboard.pbix
└── README.md
```

**Note:** Raw source tables (~20GB combined across purchases, sales, and inventory files) are not included in this repository due to size. The `vendor_sales_summary.csv` file in `data/` is the final processed output used for analysis and the Power BI dashboard.
