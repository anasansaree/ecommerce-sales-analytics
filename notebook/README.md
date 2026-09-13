# E-Commerce Sales & Customer Analytics

## Project Title
E-Commerce Sales & Customer Analytics — Synthetic Operational Dataset

## Business Objective
Simulate the operational database of a mid-sized Indian e-commerce company to support
end-to-end data analytics work: SQL-based data exploration, Python/Pandas data cleaning
and feature engineering, statistical/visual analysis with Matplotlib/Seaborn, and
executive dashboards in Power BI or Tableau. The dataset is designed to support
investigation into topics such as revenue drivers, customer acquisition performance,
retention and lifetime value, product/category profitability, discounting behavior,
payment reliability, and return/cancellation patterns.

## Dataset Description
The data is synthetic but structured to resemble a real company's imperfect
operational export — it includes realistic skew (a small number of products and
customers driving a disproportionate share of revenue), seasonality tied to Indian
shopping periods, a small amount of missing demographic data, and status-dependent
payment/return behavior. It is **not** derived from any real company or real
individuals.

## Tables

| File | Approx. Rows | Grain |
|---|---|---|
| `customers.csv` | 12,000 | One row per customer |
| `products.csv` | 500 | One row per product |
| `orders.csv` | 50,000 | One row per order |
| `order_items.csv` | ~110,000 | One row per product line within an order |
| `payments.csv` | 50,000 | One row per order (1:1 with orders) |
| `returns.csv` | ~3,500 | One row per return/refund event |
| `data_dictionary.csv` | 35 | One row per column across all tables |

## Relationships

```
customers.customer_id  ──< orders.customer_id
orders.order_id         ──< order_items.order_id
orders.order_id         ──< payments.order_id   (1:1)
orders.order_id         ──< returns.order_id
products.product_id     ──< order_items.product_id
```

Full column-level definitions, data types, and key relationships are documented in
`data_dictionary.csv`.

## Technology Stack
- **PostgreSQL** — relational storage, joins, referential integrity, data import
- **Python (Pandas)** — data cleaning, feature engineering, aggregation
- **Matplotlib / Seaborn** — exploratory and statistical visualization
- **Jupyter Notebook** — analysis workflow (`01_data_retrieval.ipynb`, `02_visualization.ipynb`)

## Project Workflow
1. **Load & validate** — imported all six CSVs into PostgreSQL, confirmed keys and
   referential integrity (0 orphan foreign keys, 0 duplicate primary keys).
2. **Clean & prepare** — converted date columns to proper datetime types, fixed the
   `age` column's float-due-to-nulls type issue, labeled missing categorical values
   (`gender`, `acquisition_channel`) as `"Unknown"` rather than dropping or guessing,
   and filled missing `age` with the median (robust to outliers).
3. **Retrieve & explore (Python + SQLAlchemy)** — connected Jupyter to PostgreSQL and
   pulled all six tables into Pandas DataFrames for analysis.
4. **Answer 16 business questions** across Revenue, Customers, Products, Discounts &
   Payments, Returns & Cancellations, and Geography (see Key Findings below).
5. **Visualize** — built supporting charts (monthly revenue trend, order status
   breakdown, customer segments, category revenue, discount impact, return reasons)
   saved to `/dashboards`.

---

## Key Findings & Recommendations

### 1. Revenue
- Total gross revenue is ~₹90.9 Cr, but only **78.55% (₹71.4 Cr) is actually
  collected** (Delivered) — the rest sits in Cancelled, Returned, or in-transit orders.
- **October shows the sharpest month-over-month jump (68–97%)** in both 2024 and
  2025, aligning with the Diwali/Dussehra festival season, with elevated sales
  continuing through December (year-end sale period).
- The business grew **~9.6x** from January 2024 to December 2025.

### 2. Customers
- **53.96%** of customers are repeat buyers — a healthy retention signal — but
  **31.17% never placed an order** after signing up.
- The top 10 customers contribute **₹1.2 Cr+** in combined lifetime value and are
  geographically diversified (no single-city concentration risk).
- Average customer LTV is consistent (**₹86k–96k**) across every acquisition
  channel — channel quality is the same, only volume differs.

### 3. Products
- **Electronics drives 64.81% of total revenue** — a significant concentration risk.
- A single product (a Bluetooth speaker) accounts for **~15% of total delivered
  revenue** on its own.
- Margins are fairly uniform by category (45–49%) but vary widely at the product
  level (30–65%), meaning profitability gains are better targeted product-by-product.

### 4. Discounts & Payments
- **Deeper discounts correlate with *lower* average order value**, not higher —
  there's no evidence heavier discounting drives bigger baskets.
- **UPI is the most-used payment method (36%)**, consistent with broader Indian
  e-commerce trends.
- Payment failure (~6–7%) and refund (~10%) rates are uniform across all payment
  methods, indicating the driver is order cancellations/returns, not gateway
  reliability.

### 5. Returns & Cancellations
- Overall return rate is **7.79%**; once normalized by category revenue, return
  rates are consistent across categories (8.1%–9.3%) — no single category has a
  systemic quality issue.
- **Damaged Product + Quality Issue account for ~41% of all returns** — both are
  operationally preventable.
- Cancellations and returns combined cost **₹12.58 Cr (13.8% of gross revenue)**.

### 6. Geography
- Mumbai and Delhi lead city-level revenue (~₹7.5–7.6 Cr each).
- Maharashtra leads state-level revenue (₹14.86 Cr), driven by the combined
  contribution of Mumbai and Pune.
- Revenue is not overly concentrated in one region — a healthy geographic spread.

### Recommendations
1. **Reduce revenue leakage** — Damaged Product and Quality Issues drive ~41% of
   returns. Tightening packaging standards and pre-shipment QC could recover an
   estimated ₹2–3 Cr of the ₹12.58 Cr lost to cancellations/returns.
2. **Re-engage dormant customers** — 31% of customers never ordered after signup.
   A targeted first-order discount campaign could convert part of this segment
   without additional acquisition spend.
3. **Reallocate marketing budget toward Organic Search** — since LTV is consistent
   across channels, prioritizing free/organic growth over Paid Search (same LTV,
   added cost) would improve marketing ROI.
4. **Rethink the discount strategy** — deep discounts (20–30%) do not increase
   average order value; targeted 5–10% discounts or bundling are likely more
   effective use of margin.

*Note: this dataset is synthetic. Findings and figures are demonstrative of the
analysis approach and reasoning, not real business results.*
