# Banking Analysis Dashboard

### 📊 [Dashboard Link](https://app.powerbi.com/groups/me/reports/your-dashboard-link-here)

## 📌 Problem Statement

This Power BI dashboard analyzes a fictional banking dataset to provide insights into customer segmentation, transaction patterns, operational strategy, and branch performance. It enables a bank to understand income group behavior, optimize product offerings, and identify trends over time and across locations.

---

## 📄 Pages Included

| Page No. | Page Name                     |
|----------|-------------------------------|
| 1        | Customer Segmentation         |
| 2        | Transaction                   |
| 3        | Operations, Trends & Strategy |
| 4–6      | TT1, TT2, TT3 (ToolTips)      |

---

## 🧩 Key Visuals and Features

### ✅ Customer Segmentation
- KPI cards for total fees, transactions, average credit score, Total Amount, Customer Monthly Income, 
- Income-level drill-down charts
- Transactions split by customer segment and income category
- Cumulative income trend over time
- Slicers for Filtering Data - by income level, channel, branch city and dynamic Currency Slicer

### ✅ Transaction Page
- Card visuals for total transactions, % Fee Value, and total Fees
- Pie chart for transaction types and product categories
- Map visual for branch performance
- Timeline of cumulative value
- Matrix showing transaction channels and types
- Slicers for Filtering Data - by income level, channel, branch city and dynamic Currency Slicer

### ✅ Operations, Trends & Strategy
- Transactions over time trend chart
- Matrix of performance by category and branch
- Average transactions per branch visual
- Slicers for Filtering Data - by Product Category and dynamic Currency Slicer

---

Added Page Navigators, Refresh Button and Information Button on all Pages. 

---

## ⚙️ DAX Measures & Expressions

Key Measures created include:
```Key Measures:
`Total Amount` = 
VAR _TargetCurr =
    SELECTEDVALUE( CurrencySlicer[Currency], "USD" )
RETURN
SUMX(
    Transactions,
    IF(
        _TargetCurr = "USD",
            IF(
                Transactions[Currency] = "USD",
                Transactions[Amount],
                Transactions[Amount] * 1.14
            ),
            -- else target = "EUR"
            IF(
                Transactions[Currency] = "EUR",
                Transactions[Amount],
                Transactions[Amount] * 0.88
            )
    )
)
// for dynamic change in currency rates.

---

Customer Monthly Income = 
VAR _TargetCurr =
    SELECTEDVALUE( CurrencySlicer[Currency], "USD" )
RETURN
SUMX(
    Transactions,
    IF(
        _TargetCurr = "USD",
            IF(
                Transactions[Currency] = "USD",
                Transactions[MonthlyIncome],
                Transactions[MonthlyIncome] * 1.14
            ),
            -- else target = "EUR"
            IF(
                Transactions[Currency] = "EUR",
                Transactions[MonthlyIncome],
                Transactions[MonthlyIncome] * 0.88
            )
    )
)

```

---

## 📊 Tables & Relationships

- **Used Tables:**
  - `Transactions` – Core financial and customer transaction data
  - `Key Measures` – Custom metrics for analysis
  - `Dates` – Calendar table
  - `CurrencySlicer` – For currency conversion or symbol formatting

- **Relationships:**
  - Dates linked with transaction date and income trend
  - BranchCity and IncomeLevel relationships to transaction data

---

## 🔍 Insights Uncovered

- High-income segments contribute to the highest cumulative income
- Transaction volume varies significantly by channel and region
- Average transaction amount and fees help evaluate customer value
- Monthly patterns suggest cyclical income inflow and spending behavior
- Branch-level analysis reveals operational and strategic opportunities

---

## 🚀 Tech Stack

- Power BI Desktop & Power BI Service  
- DAX  
- Power Query  
- ZoomCharts  
- PBI Helper for model documentation

---

📌 *Note: Pages TT1, TT2, and TT3 include placeholders for image-based analysis or future expansion.*
