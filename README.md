# Banking Analysis Dashboard

### 📊 [Dashboard Link](https://app.powerbi.com/view?r=eyJrIjoiZDkwZjQyM2UtMDJjNi00YTM3LWFmZTQtODc1ODQ3MGI3ZGY0IiwidCI6IjQ2NTRiNmYxLTBlNDctNDU3OS1hOGExLTAyZmU5ZDk0M2M3YiIsImMiOjl9)

## 📌 Problem Statement

This Power BI dashboard analyzes a fictional banking dataset to provide insights into customer segmentation, transaction patterns, operational strategy, and branch performance. It enables a bank to understand income group behavior, optimize product offerings, and identify trends over time and across locations.

---
Snap of Dashboad Pages (3) ,

![Image](https://github.com/user-attachments/assets/6a40bf45-7e4a-47df-908c-7a4e9f719e2a)

![Image](https://github.com/user-attachments/assets/4666505e-98b1-4b7f-918b-e01488d14046)

![Image](https://github.com/user-attachments/assets/110cc115-9f9c-4037-b090-3ca75afa5d1b)

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
Total Amount = 
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

---
Total Fees = 
VAR _TargetCurr =
    SELECTEDVALUE( CurrencySlicer[Currency], "USD" )

RETURN
SUMX(
    Transactions,
    VAR _RowFees =
        Transactions[CreditCardFees]
        + Transactions[InsuranceFees]
        + Transactions[LatePaymentAmount]

    RETURN
    IF(
        _TargetCurr = "USD",
            IF(
                Transactions[Currency] = "USD",
                _RowFees,                  // already USD
                _RowFees * 1.14            // EUR → USD
            ),
            // else target = "EUR"
            IF(
                Transactions[Currency] = "EUR",
                _RowFees,                  // already EUR
                _RowFees * 0.88            // USD → EUR
            )
    )
)
---
Cumulative Value = 
CALCULATE(
    [Total Amount],
    FILTER(
        ALLSELECTED(Dates),
        Dates[Date] <= MAX(Dates[Date])
    )
)
---
Cumulative Income = 
CALCULATE(
    [Customer Monthly Income],
    FILTER(
        ALLSELECTED(Dates),
        Dates[Date] <= MAX(Dates[Date])
    )
)
---
Total Customers = DISTINCTCOUNT(Transactions[CustomerID])
Total Transactions = COUNTROWS(Transactions)
Transactions per Branch = CALCULATE(    COUNTROWS(Transactions),    ALLEXCEPT(Transactions, Transactions[BranchCity]))
% Fees of Value = DIVIDE([Total Fees], [Total Amount], 0)
Avg Branch Transactions = 
AVERAGEX(    VALUES(Transactions[BranchCity]),    [Transactions per Branch])
Avg Credit Score = AVERAGE(Transactions[CustomerScore])
Cumulative Transactions = CALCULATE([Total Transactions], FILTER( ALLSELECTED(Dates), Dates[Date] <= MAX(Dates[Date])))

```
## Dynamic Currency Switch
![Image](https://github.com/user-attachments/assets/7e6a55a3-0984-4d71-8718-3e98275ec2df) ![Image](https://github.com/user-attachments/assets/3571009d-f404-4aa2-b28b-d6ccf8e02c46)

---

## 📊 Tables & Relationships

- **Used Tables:**
  - `Transactions` – Core financial and customer transaction data
  - `Key Measures` – Custom metrics for analysis
  - `Dates` – Calendar table
  - `CurrencySlicer` – For currency conversion.
  - `Currency` - Currency Table

- **Relationships:**
  - Dates linked with transaction date
  ---
- **Data Model:**
- ![Image](https://github.com/user-attachments/assets/2613df8a-b882-4112-99f5-b1dc51b8f0b4)

## 🔍 Insights Uncovered

- Mid-income and high-income segments account for the majority of total customers, but low-income segments show higher              transaction activity in certain product categories.
- Credit Cards and Savings Account are mostly used by Middle Income Level customers
- Malaga and Seville are the branches with highest transaction Value (USD)
---

## 🚀 Tech Stack

- Power BI Desktop & Power BI Service
- MS Excel for Data Source 
- DAX  
- Power Query  
- ZoomCharts Visuals
- PowerPoint and Figma for Design
- PBI Helper for documentation

---

📌 *Note: Pages TT1, TT2, and TT3 include ToolTips for the Information Button.*

![Image](https://github.com/user-attachments/assets/261456c5-667a-4e03-a1bb-d9b6d693af6c)
