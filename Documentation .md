# 📄 Documentation — Superstore Sales Dashboard

## 1. Project Aim / Goal

The goal of this project is to analyze the Sample Superstore transactional dataset (2014–2017) and build a single-page, interactive Excel dashboard that lets a business stakeholder quickly answer:

- How much are we selling and earning, overall and on average?
- Which regions, categories, segments, and shipment modes perform best/worst?
- Who are our top and bottom products and customers?
- How does performance trend over time (monthly)?

The end deliverable is a decision-ready dashboard built entirely with native Excel features (PivotTables, PivotCharts, Slicers) — no external BI software required — making it portable and easy to share.

---

## 2. Dataset Description

| Attribute | Detail |
|---|---|
| File | **[Sample_Superstore_raw_table.xls](https://github.com/DRasool7013/Superstore-Sales-Profit-Analysis-Dashboard/blob/main/Sample%20-%20Superstore_raw_table.xls)**|
| Rows | 9,994 order line items |
| Grain | One row per Order ID + Product |
| Date range | 2014 – 2017 |
| Key dimensions | Region, State, City, Segment, Category, Sub-Category, Ship Mode, Customer |
| Key measures | Sales, Quantity, Discount, Profit |
| Supporting tables | `People` — maps each Region to a Regional Manager; `Returns` — flags Order IDs that were returned |

**Original columns:** Row ID, Order ID, Order Date, Ship Date, Ship Mode, Customer ID, Customer Name, Segment, Country, City, State, Postal Code, Region, Product ID, Category, Sub-Category, Product Name, Sales, Quantity, Discount, Profit.

---

## 3. Data Cleaning & Preparation Process

1. **Imported** the raw `.xls` file and converted it into a working `.xlsx` table (`Orders` sheet).
2. **Checked for duplicates and blanks** in Order ID, Customer ID, and Product ID — none required removal beyond standard validation.
3. **Corrected data types** — Order Date and Ship Date converted to proper date format; Sales, Profit, Discount, Quantity converted to numeric.
4. **Feature engineering** — added derived columns to support analysis and slicers:
   - `MONTH`, `YEAR`, `DAY`, `DATE`, `weekday` — extracted from Order Date
   - `no. of days` — Ship Date minus Order Date (fulfillment time)
   - `DISCOUNT` / `4. DISCOUNT TYPE` — bucketed discount into categories (e.g., Low/High Discount)
   - `1. PROFIT OR LOSS` — flags each order as Profit or Loss
   - `ORDER STATUS` — flags Normal vs. other order types
   - Additional helper/segmentation columns (customer value tier, switch/IF-based classifications) used internally to power PivotTable groupings
5. **Built PivotTables** on the cleaned `Orders` sheet to power every KPI card and chart on the `KPI`, `Dashboard`, and `Charts` sheets.
6. **Validated totals** — cross-checked Total Sales, Total Profit, and Total Orders against SUM/COUNT formulas on the raw data to confirm the PivotTables matched source data exactly.

---

## 4. Analysis Performed

### A. Aggregate measures
1. **Average Sales by Category** — computed via PivotTable (Category → Rows, Sales → Values, set to Average).
2. **Minimum Sales by Customer Segment** — PivotTable (Segment → Rows, Sales → Values, set to Min).
3. **Total Sales by Category** — Technology and Office Supplies lead; Furniture trails both.
4. **Total Orders by Shipment Mode** — Standard Class carries the highest order volume, followed by Second Class, First Class, and Same Day.
5. **Total Number of Products Sold by Region** — West and East regions move the highest product volume, consistent with their sales lead.

### B. Top / Bottom rankings
6. **Top 10 Products** (by Sales) — headlined by high-ticket Technology items (e.g., copiers, phones), led by product `TEC-CO-10004722` at ~61.6K in sales.
7. **Bottom 10 Products** — lowest-selling SKUs, generally low-unit-price Office Supplies items.
8. **Top 10 Customers** (by Sales) — led by customer `SM-20320` (~25.0K) and `TC-20980` (~19.1K).
9. **Bottom 10 Customers** — lowest cumulative spend, mostly one-time or small-basket buyers.
10. **Top 10 Orders** (by Sales value) — largest single transactions, mainly bulk Technology/Furniture purchases.
11. **Bottom 10 Orders** — smallest transactions by sales value.

### C. Comparative breakdowns
12. **Sales by Region compared across Category** — clustered column chart (Region → Rows, Category → Columns, Sales → Values). Technology and Office Supplies outperform Furniture in every region; East and West are the strongest overall.
13. **Total Orders by Customer Segment compared across Shipment Mode** — clustered column chart (Segment → Rows, Ship Mode → Columns, Order ID (Count) → Values). Consumer segment dominates order volume across every ship mode.
14. **Contribution of Sales by Category to each Region** — stacked/100% column view showing each category's share of a region's total sales, used to spot regional category skew.

### D. Matrix (PivotTable) views built
| Matrix | Values | Rows | Columns |
|---|---|---|---|
| Sales × Region × Category | Sales | Region | Category |
| Orders × Segment × Ship Mode | Order ID (Count) | Segment | Ship Mode |

### E. Trend charts
15. **Sales Over Months** — line chart (Month → X-axis, Sales → Values) showing a rising trend from a mid-year dip toward a strong Q4 (Oct–Dec) close.
16. **Profit Over Days** — chart tracking daily profit fluctuation to spot loss-making days vs. high-profit days.

---

## 5. Final Result

A one-page Excel dashboard titled **"Superstore Sales Dashboard"** containing:

- **5 KPI cards**: Total Sales, Total Profit, Average Sales, Total Orders, Total Customers
- **6 charts**: Sales by Region, Sales Across Region and Category, Sales Over Months, Top 10 Products, Orders by Segment and Ship Mode, Top 10 Customers
- **5 slicers**: Ship Mode, Segment, Category, Region, Month/Year — enabling real-time cross-filtering of every chart and KPI simultaneously

See **[screenshots/Superstore_Sales_Dashboard.png](https://github.com/DRasool7013/Superstore-Sales-Profit-Analysis-Dashboard/blob/main/Superstore_Sales_Dashboard.png)** for the final rendered dashboard.

---

## 6. Outcomes

- Converted 9,994 raw, unstructured transaction rows into a governed, feature-rich data table ready for repeated analysis.
- Replaced what would be a dozen separate manual reports with a single interactive workbook.
- Delivered a reusable analysis template — the same PivotTable/slicer structure can be refreshed against a new year's data with minimal rework.

## 7. Insights

- **West** (~725K) and **East** (~679K) are the top-performing regions by sales; **South** (~392K) is the weakest, pointing to a regional growth opportunity.
- **Technology** products drive disproportionately high sales despite lower order counts than Office Supplies — a small number of high-value items (like `TEC-CO-10004722`) meaningfully move total revenue.
- **Standard Class** shipping is used for the majority of orders, suggesting most customers are not paying for expedited delivery — a possible upsell opportunity for faster shipping tiers.
- **Q4 (Oct–Dec)** consistently outperforms earlier months, confirming a seasonal/holiday sales pattern worth planning inventory and promotions around.
- Revenue concentration among the **top 10 customers and products** confirms an 80/20-style dependency — retention strategies for these accounts protect a disproportionate share of revenue.
