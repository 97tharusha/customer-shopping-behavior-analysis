# Customer Shopping Behaviour Analysis - Project Report

**Author:** Tharusha Fernando
**Type:** Guided learning project (followed a YouTube tutorial by Amlan Mohanty)
**Tools:** Python 3.13 · PostgreSQL 15 · Tableau Public
**Dataset:** 3,900 retail transactions · 19 columns
**Live Dashboard:** [View on Tableau Public](https://public.tableau.com/views/CustomerShoppingBehavior_dashboard_FINAL/ExecutiveDashboard)

---

## 1. Project Summary

This project is a **self-directed learning exercise** completed by following a guided YouTube tutorial. It walks through a complete data analytics workflow — from raw data cleaning in Python, to SQL-based business analysis in PostgreSQL, to an executive-level dashboard in Tableau.

The goal was not to produce original research, but to **practise and consolidate** the core skills required for a junior data analyst role: data wrangling, SQL querying, business-question framing, and dashboard design.

---

## 2. Learning Objectives

By completing this project, I aimed to:

- Build a Python data-cleaning pipeline with real missing-value handling
- Apply PostgreSQL for business-question analysis using joins, CTEs, and window functions
- Design a single-page executive dashboard in Tableau
- Practise translating raw numbers into business insights

---

## 3. Business Context

The fictional scenario is a US-based retail store trying to understand customer behaviour across demographics, product categories, subscription status, and shipping preferences. The analysis answers 10 business questions — from revenue by gender, to product ratings, to customer segmentation.

---

## 4. Dataset

| Attribute | Value |
|-----------|-------|
| Source | Provided by the tutorial instructor |
| Rows | 3,900 |
| Columns | 19 |
| Missing values | 37 (in `Review Rating`) |
| Key features | Demographics, purchase amount, category, subscription, shipping type, discount, previous purchases, review rating |

**Data quality caveat:** The dataset contains a synthetic correlation between `Gender` and `Discount Applied` — all Male customers have discounts applied; no Female customers do. This is an artifact of the dataset and means any gender × discount conclusion would be misleading. I flagged this rather than drawing a false insight.

---

## 5. Methodology

### 5.1 Data Cleaning (Python)

The raw dataset required three main cleaning steps:

1. **Missing value imputation** - 37 nulls in `Review Rating` were filled using the **median rating per Category**. This is a segment-aware approach — better than a global median because product categories have different rating baselines.
2. **Column standardisation** - renamed columns to snake_case (e.g., `Purchase Amount (USD)` → `purchase_amount`) for cleaner SQL and Python references.
3. **Feature engineering** - created two new columns:
   - `age_group` - quartile-based split (Young Adult / Adult / Middle-aged / Senior)
   - `purchase_frequency_days` - numeric mapping of frequency labels (Weekly = 7, Monthly = 30, Annually = 365, etc.)
4. **Redundancy check** - verified that `promo_code_used` was 100% duplicated with `discount_applied` and dropped the redundant column.

The cleaned dataset was exported to CSV and loaded into a PostgreSQL database (`customer_behavior`) using SQLAlchemy.

### 5.2 SQL Analysis (PostgreSQL)

Ten business questions were answered using SQL, covering:

- Aggregation with `GROUP BY` (revenue by gender, age group, subscription)
- Subqueries (Q2 - customers above average spend)
- Conditional aggregation (`SUM(CASE WHEN ...)`) for discount rate calculations
- CTEs (`WITH`) for customer segmentation
- Window functions (`ROW_NUMBER() OVER PARTITION BY`) for ranking products within categories
- `CASE WHEN` + `SUM() OVER (PARTITION BY ...)` for group-level percentage analysis

### 5.3 Visualisation (Tableau)

A single-page dashboard was built with five components:

| Component | Purpose |
|-----------|---------|
| KPI cards | Total revenue, customer count, average purchase, average rating |
| Revenue by Product Category | Bar chart — shows Clothing dominates |
| Revenue by Gender | Horizontal bar |
| Revenue by Subscription Status | Horizontal bar |
| Customer Purchase Frequency | Line chart — shows bimodal pattern |

---

## 6. Key Findings

- **Total Revenue:** $233,081 across 3,900 customers
- **Average Purchase:** $59.76
- **Average Rating:** 3.75 / 5
- **Category concentration:** Clothing drives **$104,264 (45%)** of total revenue, followed by Accessories ($74,200) and Footwear ($36,093)
- **Subscription gap:** Only 27% of customers are subscribers, yet they contribute $62,645 in revenue vs $170,436 from non-subscribers
- **Bimodal purchase frequency:** Two distinct customer groups - one buying every 0–7 days, another every 31+ days, with a dip in the middle

---

## 7. Business Recommendations

1. **Double down on Clothing** - nearly half of revenue comes from one category; expanding SKUs and merchandising here has the highest leverage
2. **Investigate the low subscription rate** - only 27% of customers subscribe despite frequent purchase behaviour; this suggests either a pricing issue or a marketing gap
3. **Target the 31+ day segment** - reactivation campaigns could shift occasional shoppers into the weekly cohort
4. **Verify the discount pattern** - the strong gender × discount correlation is likely a data artifact; in a real dataset, this would need investigating before acting
5. **Promote higher-margin Accessories** - second-largest category; bundling Accessories with Clothing purchases could raise average order value

---

## 8. Skills Demonstrated

| Skill | Where Applied |
|-------|---------------|
| Python data cleaning | Median imputation, dtype handling, feature engineering |
| Pandas | Groupby, transform, apply, rename |
| SQL aggregation | SUM, AVG, COUNT, GROUP BY, HAVING |
| SQL subqueries | Q2 — filtering vs average |
| SQL CTEs (`WITH`) | Q7, Q8 — segmenting and ranking |
| SQL window functions | Q8 — ROW_NUMBER OVER PARTITION BY |
| Conditional aggregation | Q6 — CASE WHEN inside SUM |
| Database connection | SQLAlchemy + psycopg2 to PostgreSQL |
| Tableau dashboard design | KPI cards, bar charts, line chart |
| Business storytelling | Recommendations derived from data |

---

## 10. What I Learned

- **The value of a repeatable workflow** - cleaning → loading → querying → visualising is a pattern I can apply to any new dataset.
- **Segment-aware imputation matters** - using per-category medians was meaningfully better than a single global median.
- **SQL window functions unlock deeper analysis** - `ROW_NUMBER() OVER (PARTITION BY ...)` was the key to the "top 3 products per category" question.
- **Dashboard design is about editing** - the hardest part of the Tableau dashboard was deciding what NOT to include.
- **Verify before you conclude** - spotting the Gender × Discount artifact showed me the importance of data quality checks before reporting insights.

---

## 12. Acknowledgements

- **Tutorial by:** Amlan Mohanty (YouTube)
- **Dataset:** Provided by the tutorial instructor
- This report was written independently to document my learning - it is not endorsed by or affiliated with the tutorial creator.

---

*This project was completed as part of my self-directed learning journey toward junior data analyst roles.*
