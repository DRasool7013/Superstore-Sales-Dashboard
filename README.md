# 📊 Superstore Sales & Profit Analysis Dashboard

An end-to-end Power BI dashboard built on the **Sample Superstore** dataset, analyzing sales, profit, orders, and customer performance across the US retail market (2014–2017).

![Dashboard Preview](Superstore_Sales_Dashboard.png)

---

## 🎯 Project Aim / Goal

The goal of this project is to analyze retail sales performance across **Region, Category, Segment, and Time**, and to build an interactive Power BI dashboard that helps business stakeholders quickly identify:
- Which regions and categories drive the most sales and profit
- Who the top-performing products and customers are
- How sales trend over months
- How orders are distributed across customer segments and shipping modes

---

## 🗂️ Repository Structure

```
Superstore-Sales-Profit-Dashboard/
│
├── README.md                          # Project overview (this file)
├── Documentation.md                   # Detailed documentation (aim, cleaning, DAX, insights)
│
├── data/
│   ├── raw/
│   │   └── Sample_-_Superstore_raw_table.xls     # Original, unmodified source dataset
│   └── cleaned/
│       └── Superstore_cleaned.xlsx               # Cleaned & transformed dataset used in Power BI
│
├── dashboard/
│   └── superstore_sales_dashboard.pbix           # Power BI dashboard file
│
└── screenshots/
    └── Superstore_Sales_Dashboard.png            # Final dashboard snapshot
```

> 💡 In this project, the README file (below) already contains the full write-up. `Documentation.md` mirrors the same content for anyone who prefers to open the deep-dive doc separately.

---

## 🧾 Dataset Description

| Detail | Value |
|---|---|
| Source file | `Sample_-_Superstore_raw_table.xls` |
| Rows | 9,994 order line items |
| Columns | 21 |
| Date range | Jan 2014 – Dec 2017 |
| Unique Orders | 5,009 |
| Unique Customers | 793 |
| Unique Products | 1,862 |
| Regions | Central, East, South, West |
| Categories | Furniture, Office Supplies, Technology |
| Sub-Categories | 17 |
| Segments | Consumer, Corporate, Home Office |
| Ship Modes | Standard Class, Second Class, First Class, Same Day |

**Key columns:** `Row ID, Order ID, Order Date, Ship Date, Ship Mode, Customer ID, Customer Name, Segment, Country, City, State, Postal Code, Region, Product ID, Category, Sub-Category, Product Name, Sales, Quantity, Discount, Profit`

---

## 🧹 Data Cleaning & Preparation

1. Imported the raw `.xls` file into Power Query.
2. Verified and fixed data types — `Order Date`/`Ship Date` → Date, `Sales`/`Profit`/`Discount` → Decimal, `Quantity` → Whole Number.
3. Checked and removed duplicate rows (based on `Row ID`).
4. Checked for nulls/blanks across all columns — none required imputation.
5. Trimmed whitespace and standardized text casing in `Customer Name`, `Product Name`, `City`, `State`.
6. Built a **Date/Calendar dimension table** (Year, Month, Month Name, Quarter) from `Order Date` for time-based analysis.
7. Built a star schema:
   - **Fact Sales** (Order line items — Sales, Profit, Quantity, Discount)
   - **Dim Product** (Product ID, Category, Sub-Category, Product Name)
   - **Dim Customer** (Customer ID, Customer Name, Segment)
   - **Dim Calendar** (Date, Month, Year, Quarter)
8. Created relationships between Fact Sales and each dimension table (1-to-many).
9. Added DAX measures for KPIs (see `Documentation.md`).

---

## 📈 KPIs (Cards)

| KPI | Value |
|---|---|
| **Total Sales** | 2,297.2K |
| **Total Profit** | 286.4K |
| **Average Sales** | 229.858 |
| **Total Orders** | 9,995 |
| **Total Customers** | 793 |

---

## 📊 Visuals / Charts Built

| Visual | Type | Description |
|---|---|---|
| **Sales by Region** | Clustered Column Chart | Total sales for Central, East, South, West |
| **Sales Across Region and Category** | Clustered Column Chart | Sales split by Region and Category (Furniture, Office Supplies, Technology) |
| **Sales Over Months** | Line Chart | Monthly sales trend (Jan–Dec) |
| **Top 10 Products** | Bar Chart | Top 10 products ranked by sales |
| **Orders by Segment and Ship Mode** | Clustered Column Chart | Order counts by Segment (Consumer, Corporate, Home Office) and Ship Mode |
| **Top 10 Customers** | Bar Chart | Top 10 customers ranked by sales |

**Slicers used:** Ship Mode, Segment, Category, Region, Month/Year

---

## 🔍 Analysis Questions Covered

1. Average sales by each category
2. Minimum sales by each customer segment
3. Total sales by each category
4. Total orders by shipment mode
5. Total number of products sold by region
6. Top 10 & Bottom 10 products
7. Top 10 & Bottom 10 customers
8. Top 10 & Bottom 10 orders
9. Sales of each region compared by category
10. Total orders of each customer segment compared by shipment mode
11. Contribution of sales by category to each region
12. Matrix view — Sales (Values), Region (Rows), Category (Columns)
13. Matrix view — Order ID (Values), Segment (Rows), Ship Mode (Columns)
14. Sales trend over months / chart
15. Profit trend over days / chart

---

## 💡 Final Result / Outcomes & Insights

- **West** and **East** are the top-performing regions by sales, while **South** trails behind.
- **Technology** and **Office Supplies** categories drive the highest sales across most regions; **Furniture** lags in profitability due to higher discounting.
- Sales show a clear **seasonal uptick toward Q4 (Nov–Dec)**, consistent with holiday-season buying.
- The **Consumer** segment places the highest volume of orders, followed by Corporate and Home Office.
- **Standard Class** is the dominant shipping mode across all segments.
- A small group of top 10 customers and top 10 products contribute disproportionately to total revenue — a classic 80/20 pattern.

---

## 🛠️ Tools Used

- **Power BI Desktop** — data modeling, DAX, dashboard design
- **Power Query** — data cleaning & transformation
- **Excel** — raw data inspection
- **GitHub** — version control & portfolio hosting

---

## 🚀 How to Use This Repository

1. Clone or download the repository.
2. Open `dashboard/superstore_sales_dashboard.pbix` in Power BI Desktop.
3. Use the slicers (Region, Category, Segment, Ship Mode, Month/Year) to filter the dashboard interactively.
4. Refer to `Documentation.md` for the full data-cleaning steps, DAX measures, and detailed insights.

---

## 📤 How to Push This Project to GitHub (Step-by-Step)

1. **Create a new repository** on GitHub → e.g. `Superstore-Sales-Profit-Dashboard` → Add a short description → Choose **Public** → Initialize with a README (or add one locally).
2. **Clone it locally:**
   ```bash
   git clone https://github.com/<your-username>/Superstore-Sales-Profit-Dashboard.git
   cd Superstore-Sales-Profit-Dashboard
   ```
3. **Add your project folders** (`data/raw`, `data/cleaned`, `dashboard`, `screenshots`) into the cloned folder.
4. **Add this README.md and Documentation.md** to the root of the repo.
5. **Stage, commit, and push:**
   ```bash
   git add .
   git commit -m "Add Superstore Sales & Profit Analysis Dashboard"
   git push origin main
   ```
6. **Verify** the README renders correctly on the GitHub repo homepage, and that the dashboard screenshot displays properly.
7. **(Optional)** Add topics/tags like `power-bi`, `data-analytics`, `dashboard`, `superstore` to improve discoverability, and pin the repo to your GitHub profile.

---

## 👤 Author

Portfolio project — Power BI Data Analytics.
