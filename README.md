# 📦 Supply Chain Inventory Analysis

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Microsoft%20Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

> A full end-to-end supply chain inventory analysis built using Excel Pivot Tables and Power BI Desktop. This project covers regional revenue performance, stockout risk detection, reorder point calibration, supplier reliability, demand forecast accuracy, promotion effectiveness, and SKU profitability.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Tools Used](#tools-used)
- [Project Structure](#project-structure)
- [Column Dictionary](#column-dictionary)
- [Derived Columns](#derived-columns)
- [Analysis Breakdown](#analysis-breakdown)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Dashboard Pages](#dashboard-pages)
- [How to Use](#how-to-use)
- [Author](#author)

---

## 🎯 Project Overview

**Business Problem:**
> The company is losing money because we lack visibility into when to reorder stock, how much inventory to keep on hand, and which products are draining our warehouse budget. We also have no clear picture of which suppliers are causing delivery delays that lead to stockouts and missed sales.

**Core Questions Answered:**
- Are we overstocking products that do not sell fast enough?
- Are we understocking products and missing sales opportunities?
- Which suppliers are reliable and which are risky?
- Can we predict demand so we order smarter?

---

## 📊 Dataset

| Property | Detail |
|---|---|
| **Source** | [Kaggle — High-Dimensional Supply Chain Inventory Dataset](https://www.kaggle.com/datasets/ziya07/high-dimensional-supply-chain-inventory-dataset) |
| **Rows** | 91,250 records |
| **Columns** | 15 original + 9 derived |
| **Date Range** | January 2024 — December 2024 |
| **Type** | Synthetic dataset for learning purposes |

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| **Microsoft Excel** | Data preparation, derived columns, pivot table analysis |
| **Power BI Desktop** | Interactive dashboard and visualization |
| **Excel Pivot Tables** | All 6 analysis phases |

---

## 📁 Project Structure

```
supply-chain-inventory-analysis/
│
├── data/
│   └── supply_chain_dataset.xlsx       # Source dataset with derived columns
│
├── powerbi/
│   └── supply_chain_dashboard.pbix     # Power BI dashboard file
│
├── docs/
│   ├── Inventory_Analysis_Plan.docx         # Initial planning document
│   └── Inventory_Analysis_Insights_Report.docx  # Final insights & recommendations
│
└── README.md
```

---

## 📖 Column Dictionary

| Column | Plain English Meaning |
|---|---|
| `Date` | Daily snapshot date of the inventory record |
| `SKU_ID` | Unique product code (Stock Keeping Unit) |
| `Warehouse_ID` | Which warehouse holds this stock |
| `Supplier_ID` | Which vendor supplies this product |
| `Region` | Geographic area (North, South, East, West) |
| `Units_Sold` | Number of units sold on that date |
| `Inventory_Level` | Units currently in stock on that date |
| `Supplier_Lead_Time_Days` | Days from order placement to delivery |
| `Reorder_Point` | Minimum stock level that triggers a new order |
| `Order_Quantity` | Number of units ordered from supplier |
| `Unit_Cost` | Cost per unit paid to supplier |
| `Unit_Price` | Selling price per unit to customers |
| `Promotion_Flag` | 1 = promo running that day, 0 = no promo |
| `Stockout_Flag` | 1 = ran out of stock, 0 = stock available |
| `Demand_Forecast` | System prediction of units expected to sell |

---

## ➕ Derived Columns

The following columns were created during the analysis phase:

| Column | Formula | Purpose |
|---|---|---|
| `Revenue` | `Units_Sold × Unit_Price` | Total revenue per record |
| `Profit` | `Unit_Price - Unit_Cost` | Margin per unit |
| `Total_Profit` | `Profit × Units_Sold` | Total profit per record |
| `Supplier_Speed` | `IF(Lead_Time ≤ 5, "Very Fast", IF(≤ 8, "Fast", "Slow"))` | Speed tier based on avg lead time of 8 days |
| `Stockout_Risk` | `IF(Inventory_Level < Reorder_Point, "Risk", "Safe")` | Proxy stockout indicator |
| `Ideal_ROP` | `Units_Sold × Supplier_Lead_Time_Days` | Theoretical reorder point |
| `ROP_Gap` | `Ideal_ROP - Reorder_Point` | Reorder calibration gap |
| `Forecast_Error` | `Demand_Forecast - Units_Sold` | Prediction accuracy per record |
| `Forecast_Accuracy` | `IF(error > 0, "Over-Predicted", IF(< 0, "Under-Predicted", "Perfect"))` | Forecast direction label |

---

## 🔍 Analysis Breakdown

### Analysis 1 — Regional Revenue
- **Objective:** Identify which region generates the most revenue and profit
- **Method:** SUM of Revenue and Profit grouped by Region
- **Key Formula:** `Revenue = Units_Sold × Unit_Price`

### Analysis 2 — Stockout Risk per Region & Warehouse
- **Objective:** Detect where inventory falls below reorder point most frequently
- **Method:** COUNT of Stockout_Risk = "Risk" grouped by Region and Warehouse_ID
- **Note:** Stockout_Flag was 0 across all records — derived proxy used instead

### Analysis 2b — Reorder Point Calibration (Root Cause)
- **Objective:** Confirm whether reorder point miscalibration causes stockout risk
- **Method:** Average ROP_Gap compared against stockout risk counts per warehouse
- **Hypothesis confirmed:** WH_3 cuts closest to ideal ROP → highest risk

### Analysis 3 — Promotion Effectiveness
- **Objective:** Determine if promotions drive real profit growth
- **Method:** Average Revenue, Units_Sold, Profit on Promotion_Flag = 1 vs 0

### Analysis 4 — Demand Forecast Accuracy
- **Objective:** Evaluate forecasting system performance
- **Method:** COUNT of Over-Predicted vs Under-Predicted vs Perfect rows

### Analysis 5 — Supplier Consistency
- **Objective:** Rank suppliers by delivery reliability using standard deviation
- **Method:** STDEV of Supplier_Lead_Time_Days per Supplier_ID

### Analysis 6 — Profit Margin by SKU
- **Objective:** Identify most and least profitable products
- **Method:** SUM of Total_Profit ranked by SKU_ID

---

## 💡 Key Findings

### 1. Regional Revenue — Balanced Portfolio
> All four regions contribute approximately 25% of total revenue each. Maximum gap between best (North) and worst (West) is only 0.34%. Regional imbalance is not a business risk.

### 2. Stockout Risk — North is Most Vulnerable
> North region carries the highest stockout risk at 5.46% with North WH_3 being the single most at-risk warehouse (267 risk records). South is the safest region at 5.11%.

### 3. Root Cause — Reorder Point Miscalibration
> Supplier lead time does NOT explain stockout risk. WH_3 warehouses set reorder points closest to the theoretical ideal leaving only a -120 unit buffer vs WH_5's -163 unit buffer. Less buffer = higher risk.

### 4. Promotions — Volume Without Profit
> Promotions drive 27.7% more units sold and 28.3% more revenue but generate ZERO additional profit. Total profit is identical at $121.51 per day regardless of promotion status.

### 5. Demand Forecasting — Essentially Guessing
> The forecasting system is correct only 0.48% of the time with a near 50/50 split between over and under predictions. North WH_4 is the most dangerous location showing consistent under-prediction at -0.080 average error.

### 6. Supplier Reliability — Speed ≠ Consistency
> SUP_7 appears fast at 7.56 days average but has the highest StdDev at 4.51 — the most unreliable supplier. SUP_4 is slower at 8.64 days but the most consistent (StdDev 3.26).

### 7. SKU Profitability — 188% Gap Between Best and Worst
> SKU_38 generates $368,631 total profit (34.9% margin) while SKU_14 generates only $127,703 (25.8% margin) despite selling similar volumes. The top 5 SKUs generate 2.2× more profit than the bottom 5.

---

## ✅ Recommendations

| Priority | Action | Impact |
|---|---|---|
| 🔴 Critical | Increase WH_3 reorder points by 120 units minimum | Directly reduces highest stockout risk locations |
| 🔴 Critical | Recalibrate demand forecasting system | Fix 0.48% accuracy rate |
| 🟠 High | Reduce SUP_7 dependency | Eliminate most variable supplier from critical orders |
| 🟠 High | Review SKU_14 and SKU_12 pricing | Bottom margin products dragging portfolio profitability |
| 🟠 High | Fix North WH_4 under-prediction | Most dangerous forecast error location |
| 🟡 Medium | Redesign promotion strategy | Promotions drive volume but zero additional profit |
| 🟡 Medium | Expand SUP_4 and SUP_5 orders | Most reliable suppliers |
| 🟢 Monitor | Increase promotion of SKU_38 and SKU_40 | Highest margin products |

---

## 📊 Dashboard Pages

| Page | Content |
|---|---|
| **Page 1 — Revenue & Profit** | KPI cards, Revenue by Region, Trend Line, Profit Matrix |
| **Page 2 — Stockout & Reorder** | Stockout Risk by Region & Warehouse, ROP Gap, Donut Chart |
| **Page 3 — Supplier & Forecast** | Supplier Consistency, Avg Lead Time, Forecast Accuracy Donut, Forecast Error by Region |
| **Page 4 — SKU Profitability** | Top 10 SKUs by Revenue, Top 10 by Profit, Bottom 5, Revenue vs Profit Scatter |

**Slicers available on all pages:**
- Region (dropdown)
- Date range (between)
- Supplier Speed (dropdown)

---

## 🚀 How to Use

### Excel Analysis
1. Download the dataset from Kaggle
2. Open in Microsoft Excel
3. Format data as a Table (`Ctrl + T`)
4. Add all derived columns listed above
5. Build pivot tables following the analysis breakdown

### Power BI Dashboard
1. Open `supply_chain_dashboard.pbix` in Power BI Desktop
2. If data does not load — go to `Home → Transform data → Data source settings`
3. Update the file path to your local Excel file
4. Click `Refresh`

---

## 👤 Author
John Butch Gromontil
May 3, 2026
**Junior Data Analyst**
SupplyChain Analytics Co.
May 2026

> This project was completed as part of a guided inventory analysis learning session covering Excel pivot table analysis through to Power BI dashboard development.

---

*Built with Excel + Power BI Desktop | Dataset from Kaggle*
