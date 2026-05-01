# 📊 ElectroHub Sales Analytics — Power BI Dashboard
 
> An end-to-end Business Intelligence project built on ElectroHub's retail data, covering data modelling, DAX measures, and interactive dashboards across 8 stakeholder-defined requirements.
 
---
 
## 🏢 About the Company
 
**ElectroHub** is a multi-category retail company selling products across 8 lines:
 
`Electronics` · `Footwear` · `Clothing` · `Home Appliances` · `Accessories` · `Kitchenware` · `Bags` · `Personal Care`
 
---
 
## 📁 Project Structure
 
```
ElectroHub-PowerBI/
│
├── powerBI_project_1.pbix          # Main Power BI report file
├── Store_Data.xlsx                 # Source data (raw + structured)
├── Power_BI_Project_1_Requirements.pptx  # Stakeholder requirements document
└── README.md
```
 
---
 
 ## 🔄 Data Pipeline
 
### Phase 1: Data Cleaning & Transformation
```
Raw Data → Identify Issues → Handle Null Values → Standardize Formats
    ↓
Remove Duplicates → Validate Data Types → Create Calculated Columns
    ↓
Ready for Modeling
```
 
**Transformations Applied:**
- Date format standardization (MM/DD/YYYY)
- Currency normalization (to standard format)
- Customer ID and Product ID validation
- Sales/Profit/Quantity field verification
- Duplicate record removal
- Outlier detection and handling
### Phase 2: Data Modeling
- Created 4 dimension tables from raw data
- Established fact table with proper grain
- Defined relationships with correct cardinality
- Built cross-dimensional relationships
### Phase 3: Dashboard Development
- Designed interactive visualizations
- Implemented DAX measures for KPIs
- Added filters and slicers for drill-through capability
- Applied conditional formatting for insights
- Optimized performance using Edit Interactions
---

## 🗃️ Data Model
 
The project follows a **Star Schema** with one Fact table and three Dimension tables.
 
### Tables
 
| Table | Type | Rows | Description |
|---|---|---|---|
| `Sheet3` (Fact Sales) | Fact | 3,510 | Transactional records with sales, discount, and net sales |
| `Dim Customers` | Dimension | 50 | Customer info — name, city, state, pincode, email, phone |
| `Dim Product` | Dimension | 30 | Product catalogue with product line and price (INR) |
| `Dim Promotion` | Dimension | 5 | Promotion types, ad channels, coupon codes, and discount types |
 
### Relationships (Cardinality)
 
```
Dim Customers  (1) ──── (*)  Fact Sales   [CustomerID]
Dim Product    (1) ──── (*)  Fact Sales   [Product ID]
Dim Promotion  (1) ──── (*)  Fact Sales   [PromotionID]
```
 
> **Key Learning:** Dim tables are connected **only through the Fact table** — direct Dim-to-Dim relationships are avoided to maintain a clean star schema and prevent ambiguous filter paths.
 
### Fact Table Columns
 
| Column | Description |
|---|---|
| Date | Transaction date (2020–2024) |
| CustomerID | Foreign key → Dim Customers |
| PromotionID | Foreign key → Dim Promotion (0 = no promotion) |
| Product ID | Foreign key → Dim Product |
| Units Sold | Quantity sold per transaction |
| Price Per Unit | Unit price (INR) |
| Total Sales | Gross revenue before discount |
| Discount Percentage | % discount applied |
| Discount Value | Absolute discount amount |
| Net Sales | Revenue after discount |
 
---
 
## 📋 Stakeholder Requirements & Implementation
 
### Requirement 1 — Top / Bottom 5 Products
**"Top/Bottom 5 products by Sales / Profit / Quantity Sold"**
 
- Used **RANKX** DAX function to dynamically rank products
- Visual: Bar chart with a slicer to toggle between Top 5 / Bottom 5 and between Sales / Profit / Quantity
- Supports drill-through for product-level detail
---
 
### Requirement 2 — Sales Trends Over Time
**"How do sales trends vary over time — daily, monthly, quarterly, annually?"**
 
- Built a **Date Dimension** table with hierarchy: Year → Quarter → Month → Day
- Visual: Line chart with drill-down enabled across the date hierarchy
- Users can toggle granularity from a single chart
---
 
### Requirement 3 — Sales vs Profit Relationship
**"Show the relationship between Sales and Profit"**
 
- Visual: Scatter plot — Sales on X-axis, Profit on Y-axis, each bubble = one product
- Helps identify high-revenue but low-margin products at a glance
---
 
### Requirement 4 — Period Comparison
**"Compare Sales / Profit / Quantity Sold between any two user-selected periods"**
 
- Implemented using two independent **Date Range Slicers** (Period A and Period B)
- DAX measures created for each period using `CALCULATE` + `DATESBETWEEN`
- Visual: Clustered bar chart — Period A vs Period B side by side
---
 
### Requirement 5 — Average Discount by Category
**"Average discount offered in each discount category"**
 
- Promotion categories pulled from `Dim Promotion` (Summer Sale, Diwali, New Year Special, Weekend Flash Sale, Clearance Sale)
- Visual: Column chart showing average Discount % per promotion type
- Includes "No Promotion" (PromotionID = 0) as a separate category
---
 
### Requirement 6 — Total Number of Orders
**"Total number of orders"**
 
- DAX measure: `Total Orders = COUNTROWS('Fact Sales')`
- Displayed as a KPI Card on the overview page
- Filterable by product, date, customer, and promotion via slicers
---
 
### Requirement 7 — Order-Level Detail Table
**"Show Sales / Profit / Discount / Net Sales / All remaining fields for each order, filterable by Product / Date / Customer ID / Promotion Categories"**
 
- Visual: Matrix / Table with all order-level fields
- Filters: Product slicer, Date range slicer, Customer ID slicer, Promotion Category slicer
> **Key Learning for Req 7:** To filter a Dim table (e.g., Dim Product) and have it affect another Dim table's visuals, a **direct relationship between Dim tables is not valid** in a star schema. The fix was to use **Edit Interactions** in Power BI to control which visuals respond to which slicers — no bridge tables or model hacks needed.
 
---
 
### Requirement 8 — Sales by City
**"Show Sales by different cities"**
 
- Customer city data joined from `Dim Customers`
- Visual: Filled Map (ArcGIS / built-in map) and a bar chart backup
- Cities span Maharashtra, Gujarat, MP, and other Indian states
---
 
## 🛠️ Tools & Techniques Used
 
| Area | Details |
|---|---|
| **Tool** | Microsoft Power BI Desktop |
| **Data Source** | Excel (.xlsx) — Star Schema |
| **Data Cleaning** | Power Query Editor — removed nulls, standardized dates, typed columns |
| **Data Modelling** | Star schema — 1 Fact + 3 Dim tables, cardinality set explicitly |
| **DAX** | RANKX, CALCULATE, DATESBETWEEN, COUNTROWS, SUMX, AVERAGEX |
| **Visuals Used** | Bar/Column charts, Line chart, Scatter plot, KPI Cards, Matrix, Map, Slicers |
| **Interactions** | Edit Interactions used to manage cross-filter behaviour between Dim-level slicers |
 
---
 
## 🔑 Key Learnings
 
1. **Measures vs Calculated Columns** — Started with calculated columns; refactored to measures to reduce `.pbix` file size and improve performance.
2. **Star Schema Constraints** — Dim-to-Dim relationships cause ambiguity; all filters must flow through the Fact table.
3. **Edit Interactions** — A powerful alternative to adding extra relationships; lets you surgically control which visuals respond to a slicer.
4. **Date Hierarchy** — Building a proper Date table unlocks time intelligence functions in DAX (YTD, QTD, period comparison).
---
 
## 📊 Dataset Overview
 
| Property | Value |
|---|---|
| Date Range | Jan 2020 – Jan 2024 |
| Total Transactions | 3,510 |
| Customers | 50 |
| Products | 30 |
| Promotion Types | 5 + No Promotion |
| Product Lines | 8 |
| Geography | Pan-India (cities across multiple states) |
 
---
 
## 🚀 How to Open
 
1. Download and install **[Power BI Desktop](https://powerbi.microsoft.com/desktop/)** (free)
2. Clone or download this repository
3. Open `powerBI_project_1.pbix` in Power BI Desktop
4. The data is embedded — no external connection setup needed
---
 
## 👤 Author
 
**[Uta Harish Kumar]**
📧 [utaharish96@gmail.com]
🔗 [https://www.linkedin.com/in/utaharishkumar/]
🐙 [https://github.com/Harish-Uta17]
 
---
 
