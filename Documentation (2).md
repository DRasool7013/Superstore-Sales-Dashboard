# 📄 Documentation — Superstore Sales & Profit Analysis Dashboard

---

## 1. Project Aim / Goal

To build an interactive Power BI dashboard that analyzes the Sample Superstore retail dataset and answers key business questions around **sales performance, profitability, customer behavior, and order fulfillment**, so that stakeholders can make faster, data-driven decisions on regional strategy, product focus, and shipping operations.

---

## 2. Dataset Description

| Detail | Value |
|---|---|
| File name | `Sample_-_Superstore_raw_table.xls` |
| Total rows | 9,994 |
| Total columns | 21 |
| Date range | January 2014 – December 2017 |
| Grain | One row per order line item (an order can contain multiple line items) |

### Column Dictionary

| Column | Description |
|---|---|
| Row ID | Unique row identifier |
| Order ID | Unique order identifier (5,009 unique orders) |
| Order Date | Date the order was placed |
| Ship Date | Date the order was shipped |
| Ship Mode | Standard Class, Second Class, First Class, Same Day |
| Customer ID / Customer Name | Unique customer identifiers (793 unique customers) |
| Segment | Consumer, Corporate, Home Office |
| Country / City / State / Postal Code | Location of the order |
| Region | Central, East, South, West |
| Product ID / Product Name | Unique product identifiers (1,862 unique products) |
| Category | Furniture, Office Supplies, Technology |
| Sub-Category | 17 sub-categories (e.g., Chairs, Binders, Phones) |
| Sales | Revenue generated from the line item |
| Quantity | Units sold |
| Discount | Discount rate applied |
| Profit | Profit generated from the line item |

---

## 3. Data Cleaning Process

Performed in **Power Query** before loading into the Power BI data model:

1. **Import** — Loaded the raw `.xls` file into Power Query Editor.
2. **Type correction** — Set `Order Date`/`Ship Date` to Date, `Sales`/`Profit`/`Discount` to Decimal Number, `Quantity` to Whole Number, and IDs/text fields to Text.
3. **Duplicate check** — Verified `Row ID` uniqueness; no duplicate rows found.
4. **Null/blank check** — Scanned all columns for blanks; dataset was complete with no missing values requiring imputation.
5. **Text standardization** — Trimmed leading/trailing spaces and standardized capitalization in `Customer Name`, `Product Name`, `City`, and `State`.
6. **Calendar table** — Created a dedicated Date dimension (Year, Month Number, Month Name, Quarter) using `CALENDAR()` / `CALENDARAUTO()` DAX, based on the min/max of `Order Date`.
7. **Star schema modeling** — Split the flat file into a Fact + Dimension model:
   - `Fact_Sales` → Order Date, Ship Date, Ship Mode, Sales, Quantity, Discount, Profit, keys to dimensions
   - `Dim_Product` → Product ID, Product Name, Category, Sub-Category
   - `Dim_Customer` → Customer ID, Customer Name, Segment
   - `Dim_Calendar` → Date, Month, Month Name, Quarter, Year
8. **Relationships** — One-to-many relationships from each Dim table to `Fact_Sales`, all set to single-direction filtering for predictable slicer behavior.
9. **Load** — Loaded the cleaned/modeled tables into the Power BI data model for visualization.

---

## 4. DAX Measures Used

```DAX
Total Sales = SUM ( Fact_Sales[Sales] )

Total Profit = SUM ( Fact_Sales[Profit] )

Average Sales = AVERAGE ( Fact_Sales[Sales] )

Total Orders = DISTINCTCOUNT ( Fact_Sales[Order ID] )

Total Customers = DISTINCTCOUNT ( Fact_Sales[Customer ID] )

Total Quantity Sold = SUM ( Fact_Sales[Quantity] )

Profit Margin % = DIVIDE ( [Total Profit], [Total Sales], 0 )
```

> Note: The "Total Orders" KPI card in the dashboard displays 9,995 based on the count used at build time (order line-item level aggregation); `DISTINCTCOUNT(Order ID)` on the raw data returns 5,009 unique orders — both figures are documented here for transparency.

---

## 5. KPI Cards

| KPI | Measure | Value |
|---|---|---|
| Total Sales | `[Total Sales]` | 2,297.2K |
| Total Profit | `[Total Profit]` | 286.4K |
| Average Sales | `[Average Sales]` | 229.858 |
| Total Orders | `[Total Orders]` | 9,995 |
| Total Customers | `[Total Customers]` | 793 |

---

## 6. Visuals & Chart Design

| # | Visual | Chart Type | Fields |
|---|---|---|---|
| 1 | Sales by Region | Clustered Column | Axis: Region · Values: Sales |
| 2 | Sales Across Region and Category | Clustered Column | Axis: Region · Legend: Category · Values: Sales |
| 3 | Sales Over Months | Line Chart | Axis: Month (Order Date) · Values: Sales |
| 4 | Top 10 Products | Bar Chart (Top N filter) | Axis: Product · Values: Sales |
| 5 | Orders by Segment and Ship Mode | Clustered Column | Axis: Segment · Legend: Ship Mode · Values: Count of Order ID |
| 6 | Top 10 Customers | Bar Chart (Top N filter) | Axis: Customer · Values: Sales |

**Matrix visuals (from analysis notes):**
- Matrix A — Values: Sales · Rows: Region · Columns: Category
- Matrix B — Values: Order ID (count) · Rows: Segment · Columns: Ship Mode

**Slicers:** Ship Mode, Segment, Category, Region, Month/Year — placed at the top of the report page for global filtering.

---

## 7. Business Questions Answered

1. Average sales by each category
2. Minimum sales by each customer segment
3. Total sales by each category
4. Total orders by shipment mode
5. Total number of products sold by region
6. Top 10 and Bottom 10 products
7. Top 10 and Bottom 10 customers
8. Top 10 and Bottom 10 orders
9. Sales of each region compared across category
10. Total orders of each customer segment compared by shipment mode
11. Contribution of sales by category to each region
12. Sales trend over months (line chart)
13. Profit trend over days (line chart)

---

## 8. Final Result

A single-page, interactive Power BI dashboard titled **"Superstore Sales Dashboard"** with 5 KPI cards and 6 supporting visuals, fully cross-filterable through 5 slicers, enabling drill-down analysis by region, category, segment, ship mode, and time period.

---

## 9. Outcomes & Insights

- **Regional performance:** West (725K) and East (679K) generate the highest sales, followed by Central (501K) and South (392K) — the South region represents the biggest growth opportunity.
- **Category mix:** Technology and Office Supplies consistently outperform Furniture across all four regions; Furniture's higher discount rates compress its profit margin.
- **Seasonality:** Monthly sales trend shows a dip in early spring, gradual growth through mid-year, and a strong peak in November–December, indicating a holiday/year-end sales spike worth planning inventory and promotions around.
- **Customer concentration:** The top 10 customers (e.g., TC-20980, TA-21385, SM-20320) contribute a disproportionate share of revenue relative to the customer base of 793 — a retention/loyalty program could protect this high-value segment.
- **Product concentration:** A small set of top 10 products (led by copier and high-value office/tech items) account for a large share of total sales, suggesting focused inventory and marketing investment on these SKUs.
- **Segment & shipping behavior:** The Consumer segment places the most orders overall, and Standard Class shipping dominates across all three segments — indicating cost-conscious buying behavior and an opportunity to test faster-shipping promotions to lift average order value.

---

## 10. Tools & Technologies

- Power BI Desktop (data modeling, DAX, visualization)
- Power Query (ETL / data cleaning)
- Microsoft Excel (source data review)
- GitHub (version control, portfolio hosting)

---

## 11. Repository & Publishing Checklist

- [x] Raw dataset added (`data/raw/`)
- [x] Cleaned dataset added (`data/cleaned/`)
- [x] `.pbix` dashboard file added (`dashboard/`)
- [x] Dashboard screenshot added (`screenshots/`)
- [x] README.md written
- [x] Documentation.md written
- [ ] Repository pushed to GitHub
- [ ] Repo description, topics, and pinned status set on GitHub profile
