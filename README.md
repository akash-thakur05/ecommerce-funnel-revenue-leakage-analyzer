# 🛒 E-Commerce Funnel & Revenue Leakage Analyzer
### Olist Brazilian E-Commerce · Python · Pandas · Matplotlib · Seaborn · JupyterLab

---

## 📌 Project Overview

This project analyses **99,441 real orders** from Olist — Brazil's largest e-commerce marketplace — to answer one business question:

> **Where is revenue being lost in the purchase funnel, and why?**

Starting from raw CSV files, the analysis builds a complete purchase funnel, identifies failed orders, quantifies leakage in R$, and breaks it down by geography, product category, and time — producing 6 charts and a final executive summary.

---

## 🔑 Key Findings

| Metric | Value |
|---|---|
| Period covered | Sep 2016 – Oct 2018 |
| Total orders analysed | 99,441 |
| Total revenue collected | R$16,008,872 |
| Average order value | R$160.99 |
| Failed orders (cancelled + unavailable) | 1,234 (1.24%) |
| Estimated revenue leakage | **R$198,662** |
| Orders cancelled after payment approval | **77.4%** (484 of 625) |
| Worst state by loss rate | Rondônia — RO (2.77%) |
| Worst category by loss rate | livros_interesse_geral (general books) |
| Peak leakage months | September & October |

---

## 💡 Key Insights

**1. Most cancellations happen before the product ever ships.**
619 of 625 cancelled orders (99%) have no delivery date — they were cancelled before any fulfilment attempt. This means the problem is seller inventory management, not the logistics network.

**2. 77.4% of cancellations happen after payment approval.**
484 customers had their payment approved and then got cancelled. This is the most damaging kind — the customer trusted the platform with their money and still didn't receive their order.

**3. "Unavailable" orders are a separate, hidden problem.**
609 orders were marked "unavailable" — sellers listed products they couldn't actually fulfil. This is distinct from customer-initiated cancellations and points to a platform-side seller vetting failure.

**4. The 6 cancelled orders that DID ship averaged 19.8 days delivery time** — vs 12.1 days for successful deliveries. These were likely cancelled because the customer gave up waiting.

**5. September and October show consistently higher loss rates** across 2017 and 2018, likely driven by sellers over-listing ahead of the holiday season without adequate stock.

---

## 📋 Recommendations

1. **Audit sellers in the `livros_interesse_geral` category** — highest loss rate among all categories with 100+ orders
2. **Require inventory verification before a product listing goes live** — would directly reduce the 609 "unavailable" orders
3. **Auto-flag sellers with a cancellation rate above 2%** — Rondônia (RO) at 2.77% is above this threshold
4. **Auto-cancel unapproved orders after 24 hours** — reduces orders stuck in "processing" with no resolution
5. **Tighten inventory checks in August** — before the Sep/Oct peak leakage window each year

---

## 🗂️ Project Structure

```
ecommerce-funnel/
├── data/                              ← raw CSVs (never modified)
│   ├── olist_orders_dataset.csv
│   ├── olist_order_items_dataset.csv
│   ├── olist_customers_dataset.csv
│   ├── olist_order_payments_dataset.csv
│   └── olist_products_dataset.csv
│
├── notebooks/
│   ├── 01_exploration.ipynb           ← data loading, profiling, first chart
│   ├── 02_funnel_analysis.ipynb       ← date math, table merges, funnel chart
│   └── 03_revenue_leakage.ipynb       ← state, category, time breakdowns
│
└── notebooks/outputs/
    ├── 03_conversion_funnel.png       ← purchase funnel (5 stages)
    ├── 04_revenue_leakage.png         ← leakage by order status
    ├── 05_leakage_by_state.png        ← geographic breakdown (count + rate)
    ├── 06_leakage_by_category.png     ← top 15 categories by loss rate
    ├── 07_leakage_over_time.png       ← monthly trend (orders + loss rate)
    └── 08_heatmap_loss_rate.png       ← loss rate heatmap (month × year)
```

---

## 📓 Notebook Summary

### `01_exploration.ipynb` — Data Loading & Profiling
- Loads 4 CSV files (orders, items, customers, payments)
- Confirms data integrity: 99,441 rows, correct dtypes
- Identifies missing values: 2,965 orders with no delivery date
- Finds total revenue: R$16,008,872
- Produces first bar chart: order count by status

### `02_funnel_analysis.ipynb` — Funnel & Leakage Quantification
- Converts 5 text date columns to real `datetime64` objects
- Calculates delivery time per order (mean: 12.5 days, median: 10.0 days)
- Merges orders + payments using `groupby` + `merge`
- Builds revenue-by-status table (cancelled/unavailable = R$0 collected)
- Draws the 5-stage conversion funnel chart
- Estimates R$198,662 in total leakage
- Diagnoses: 77.4% of cancellations occurred after payment approval

### `03_revenue_leakage.ipynb` — Deep Dive by Dimension
- Merges 5 tables into one 99,441-row master DataFrame
- **WHERE:** Rondônia (RO) worst loss rate at 2.77%; SP highest raw count
- **WHAT:** `livros_interesse_geral` highest loss rate among 73 categories
- **WHEN:** Sep/Oct consistently worst months; 2016 data excluded (small sample)
- Produces heatmap (loss rate by month × year)
- Prints full executive summary

---

## 🛠️ Tech Stack

| Tool | Version | Purpose |
|---|---|---|
| Python | 3.13 | Core language |
| Pandas | latest | Data loading, merging, grouping |
| Matplotlib | latest | Chart drawing and saving |
| Seaborn | latest | Styled charts and heatmap |
| NumPy | latest | Numeric operations |
| JupyterLab | 4.5.7 | Interactive notebook environment |

---

## 📦 Dataset

**Source:** [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) — Kaggle

9 CSV files covering orders, products, sellers, customers, payments, reviews, and geolocation from Sep 2016 to Oct 2018. This project uses 5 of the 9 files.

---

## ▶️ How to Run

```bash
# 1. Clone or download the project folder
# 2. Download the dataset from Kaggle and place CSVs in data/
# 3. Install dependencies
pip install pandas matplotlib seaborn numpy jupyter

# 4. Launch JupyterLab from the project root
cd ecommerce-funnel
jupyter lab

# 5. Run notebooks in order: 01 → 02 → 03
```

> **Note:** Update the `DATA` path variable in notebooks 02 and 03 to match your local machine path.

---

## 👤 Author

**Akash** — Data Analyst in training  
Project built from scratch as part of a structured data analytics portfolio.  
Dataset: Olist Brazilian E-Commerce (Kaggle) · Tools: Python, Pandas, Matplotlib, Seaborn
