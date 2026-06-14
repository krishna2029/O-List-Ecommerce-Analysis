# 🛒 Olist Brazilian E-Commerce — End-to-End Data Analysis

## 📌 Project Overview

End-to-end data analysis project on **Olist**, Brazil's largest e-commerce marketplace.
The analysis covers **94,480 orders** across **2016–2018**, examining revenue trends,
delivery performance, customer behaviour, and seller risk to produce actionable
business recommendations for leadership.

> **Role:** Data Analyst (simulated business scenario)  
> **Goal:** Identify what is driving revenue and satisfaction — and what operational
> changes will improve growth and retention over the next 2 quarters.

---

## 📂 Repository Structure

```
olist-ecommerce-analysis/
│
├── 📁 notebooks/
│   └── olist_analysis.ipynb        # Full EDA notebook (Python)
│
├── 📁 dashboard/
│   ├── olist_dashboard.pbix        # Power BI dashboard file
│       ├── page1_executive_summary.png
│       ├── page2_delivery_operations.png
        ├── page3_Customers.behaviour.png
│       └── page4_seller_performance.png
│
├── 📁 reports/
│   └── olist_analysis_report.pdf   # Full written analysis report
├── 📁 data/
│   └── README.md                   # Dataset info and download link
│
└── README.md
```

---

## 🗂️ Dataset

| Property | Details |
|---|---|
| **Source** | [Olist Brazilian E-Commerce — Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) |
| **Period** | December 2016 — August 2018 |
| **Size** | 100K+ orders across 9 CSV files |
| **Tables** | Orders, Customers, Products, Sellers, Payments, Reviews, Geolocation |

> ⚠️ Raw CSV files are not uploaded due to size. Download directly from Kaggle using the link above.

---

## 🛠️ Tools & Technologies

| Tool | Version | Purpose |
|---|---|---|
| Python | 3.10 | Data cleaning, EDA, visualisation |
| Pandas | 2.x | Data manipulation and aggregation |
| Matplotlib & Seaborn | Latest | Charts and statistical plots |
| Power BI Desktop | Latest | Interactive 4-page dashboard |

---

## ❓ Business Problem

> *"Which product categories, states, and seller segments are driving Olist's revenue
> and customer satisfaction — and what operational changes will improve growth
> and retention over the next 2 quarters?"*

---

## 🔄 Project Workflow

```
Raw CSV Files (9 tables)
        ↓
Data Assessment — understand structure, identify issues
        ↓
Merging — joined all tables into one master dataframe
        ↓
Cleaning — datetime conversion, null handling, removed
           cancelled orders, filtered invalid payments
        ↓
Feature Engineering — delivery_time, is_late flag,
                      revenue column, order_month/year
        ↓
EDA (Python) — 4 analysis sections with 20+ charts
        ↓
Power BI Dashboard — 4 interactive pages
        ↓
Business Recommendations — data-backed action plan
```

---

## 📊 Key Metrics

| Metric | Value |
|---|---|
| Total Orders | 94,480 |
| Unique Customers | 91,473 |
| Total Revenue | R$ 13,732,498 |
| Average Review Score | 4.16 / 5.0 |
| Average Delivery Time | 12.0 days |
| On-Time Delivery Rate | 90.9% |

---

## 🔍 Analysis Sections

### 1. Sales & Revenue Analysis
- Monthly revenue trend (2016–2018)
- Month-over-Month revenue growth %
- Top 10 and Bottom 10 product categories by revenue
- Average Order Value (AOV) trend
- Payment type distribution and installment behaviour

### 2. Delivery & Operations Analysis
- Delivery time distribution (histogram with mean/median)
- Average delivery time by state
- On-time vs late delivery rate overall and by state
- Impact of late delivery on review scores
- Monthly late delivery trend

### 3. Customer Behaviour Analysis
- Top cities and states by customer volume and revenue
- Repeat purchase rate
- Review score distribution
- Average review score by product category and state
- Correlation matrix — delivery time, payment value, review score

### 4. Seller Analysis
- Top 10 sellers by revenue
- Seller summary table (orders, revenue, review, late rate)
- Revenue vs review score scatter — risk quadrant identification
- Late delivery rate by seller

---

## 💡 Key Findings

### Finding 1 — Revenue Concentration
The top 3 categories — **health_beauty (R$1.45M), watches_gifts (R$1.31M),
and bed_bath_table (R$1.30M)** — contribute **25.6% of total platform revenue**
despite being 3 out of 70+ categories.

### Finding 2 — Delivery Crisis in Northern States
**9.1% of orders are delivered late.** States AL (26.4%), MA (20.6%), and PI (17.7%)
have the worst on-time rates. Northern states RR, AP, and AM wait an average of
**25–29 days** — more than double the national average of 12 days.

### Finding 3 — Late Delivery Directly Destroys Satisfaction
Late orders receive an average review score of **2.80** vs **4.21** for on-time orders —
a drop of **1.41 points**. This is the single strongest correlation in the dataset.

### Finding 4 — Critical Customer Retention Gap
Only **3.0% of customers make a repeat purchase.** 97% of customers buy once and
never return — indicating a major loyalty and post-purchase experience gap.

### Finding 5 — Risk Sellers
**82 sellers** are classified as risk sellers (above-median revenue + avg review < 3.0).
These sellers handle **1.1% of total orders** while actively pulling down platform
satisfaction scores.

---

## 📋 Business Recommendations

| # | Area | Recommendation | Priority |
|---|---|---|---|
| 1 | Revenue | Increase marketing spend and inventory depth in top 3 categories — highest ROI potential | 🔴 High |
| 2 | Delivery | Partner with regional logistics providers in AL, MA, PI states — worst on-time rates | 🔴 High |
| 3 | Delivery | Set state-level SLA targets — northern states need dedicated fulfilment strategy | 🟠 Medium |
| 4 | Retention | Introduce post-purchase email sequence with discount on second order — 97% don't return | 🔴 High |
| 5 | Sellers | Implement Seller Quality Score — give 82 risk sellers a 90-day improvement plan | 🟠 Medium |

---

## 📈 Dashboard Preview

### Page 1 — Executive Summary
![Executive Summary](dashboard/screenshots/page1_executive_summary.png)

### Page 2 — Sales & Products
![Sales and Products](dashboard/screenshots/page2_sales_products.png)

### Page 3 — Delivery & Operations
![Delivery and Operations](dashboard/screenshots/page3_delivery_operations.png)

### Page 4 — Seller Performance
![Seller Performance](dashboard/screenshots/page4_seller_performance.png)

---

## 📝 SQL Highlights

Key SQL queries written for this project (see `sql/olist_queries.sql`):

```sql
-- Month-over-Month revenue growth using LAG()
SELECT order_month,
       ROUND(SUM(revenue), 2) AS monthly_revenue,
       ROUND((SUM(revenue) - LAG(SUM(revenue)) OVER (ORDER BY order_month))
             * 100.0 / LAG(SUM(revenue)) OVER (ORDER BY order_month), 2) AS growth_pct
FROM olist_master
GROUP BY order_month
ORDER BY order_month;

-- Risk sellers: high revenue, low satisfaction
SELECT seller_id, total_orders, total_revenue, avg_review
FROM seller_stats
WHERE total_revenue > (SELECT MEDIAN(total_revenue) FROM seller_stats)
AND avg_review < 3.0
ORDER BY total_revenue DESC;
```

---

## 🚀 How to Run This Project

1. Clone the repository
```bash
git clone https://github.com/yourusername/olist-ecommerce-analysis.git
cd olist-ecommerce-analysis
```

2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn kagglehub
```

3. Download the dataset
```bash
# The notebook uses kagglehub to auto-download
# Or manually download from:
# https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
```

4. Run the notebook
```bash
jupyter notebook notebooks/olist_analysis.ipynb
```

5. Open the dashboard
- Open `dashboard/olist_dashboard.pbix` in Power BI Desktop

---

## 👤 Author

**[Your Name]**  
B.Tech Computer Science | Aspiring Data Analyst  
📧 [your.email@gmail.com]  
🔗 [LinkedIn Profile](https://linkedin.com/in/yourprofile)  
🐙 [GitHub Profile](https://github.com/yourusername)

---

*If you found this project useful, please ⭐ star the repository!*
