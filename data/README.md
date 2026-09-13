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
- **PostgreSQL** — relational storage, joins, window functions, cohort/RFM-style SQL queries
- **Python (Pandas)** — data cleaning, feature engineering, aggregation
- **Matplotlib / Seaborn** — exploratory and statistical visualization
- **Power BI / Tableau** — interactive dashboards and executive reporting

## Basic Project Workflow
1. **Load & validate** — import the six CSVs into PostgreSQL (or Pandas), confirm keys
   and referential integrity.
2. **Clean & prepare** — handle the small pockets of missing demographic data, cast
   date columns, derive helper fields (e.g., order month, customer tenure).
3. **Explore (SQL / Pandas)** — investigate revenue trends, customer segments, product
   performance, and order/payment/return behavior.
4. **Visualize (Matplotlib / Seaborn)** — build supporting charts for the analysis.
5. **Dashboard (Power BI / Tableau)** — assemble key metrics into an interactive report.

*Analysis, insights, and conclusions are intentionally left for the analyst to
discover — this dataset only provides the raw operational tables.*
