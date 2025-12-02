# Product Profitability by Category — BI Project

A comprehensive Business Intelligence project for analyzing product profitability across categories and over time.

## Objective

Identify which product categories generate the highest profit and visualize performance trends to drive business decisions.

---

## Project Steps

### SQL VIEW Creation

A unified SQL VIEW (`vw_ProductProfitabilityFull`) joins all relevant tables:

- **FactResellerSales**
- **DimProduct**
- **DimProductSubcategory**
- **DimProductCategory**
- **DimDate**
- **DimReseller**
- **DimGeography**

This VIEW provides:

- Product & category info
- Sales & profit figures
- Time dimension for trend analysis
- Geographic breakdown

#### SQL VIEW Code

```sql
CREATE VIEW vw_ProductProfitabilityFull AS
SELECT 
    frs.ProductKey,
    p.EnglishProductName AS ProductName,
    sc.EnglishProductSubcategoryName AS Subcategory,
    c.EnglishProductCategoryName AS Category,

    frs.ResellerKey,
    g.EnglishCountryRegionName AS Country,
    g.StateProvinceName AS State,
    g.City AS City,

    frs.OrderDateKey,
    d.FullDateAlternateKey AS OrderDate,
    d.CalendarYear,
    d.MonthNumberOfYear,
    d.EnglishMonthName,

    frs.SalesAmount,
    frs.TotalProductCost,
    (frs.SalesAmount - frs.TotalProductCost) AS Profit,
    CASE 
        WHEN frs.SalesAmount = 0 THEN 0
        ELSE (frs.SalesAmount - frs.TotalProductCost) / frs.SalesAmount
    END AS ProfitMargin
FROM FactResellerSales frs
    JOIN DimProduct p ON frs.ProductKey = p.ProductKey
    JOIN DimProductSubcategory sc ON p.ProductSubcategoryKey = sc.ProductSubcategoryKey
    JOIN DimProductCategory c ON sc.ProductCategoryKey = c.ProductCategoryKey
    JOIN DimDate d ON frs.OrderDateKey = d.DateKey
    JOIN DimReseller r ON frs.ResellerKey = r.ResellerKey
    JOIN DimGeography g ON r.GeographyKey = g.GeographyKey;
```

---

###  Power BI Dashboard Design

#### KPI: Total Profit

- **Indicator:**  
  `SUM(vw_ProductProfitability[Profit])`
- **Trend Axis:**  
  `vw_ProductProfitability[OrderDate]`
- **Goal / Target Line (optional):**  
  Manually set, e.g. `500000`
- **Color Coding:**  
  - Green: Profit increasing
  - Red: Profit below expectations
  - Yellow: Moderate/stable trend

#### Line Chart — Profit Trend Over Time

- **X-Axis:** `vw_ProductProfitability[OrderDate]`
- **Y-Axis:** `SUM(vw_ProductProfitability[Profit])`
- **Legend (optional):** `vw_ProductProfitability[Category]`
- This chart visualizes monthly profit changes and category trends.

#### Bar Chart — Profit by Category

- **Axis:** Category
- **Values:** `SUM(Profit)`
- Easily compare outperformance among categories.

---

### Screenshots

KPI Example

![KPI Cards]  (https://bit.ly/BI-Project-KPIDiscount)

Dashboard Overview
![Dashboard Overview]  (https://bit.ly/BI-Project-DashboardOverview)

Bar Chart
![Bar Chart]  (https://bit.ly/BI-Project-Bar-Chart)

Profit Line Chart 
![Profit Line Chart ]  (https://bit.ly/BI-Project-ProfitLineChart)

Slicer
![Slicer ]  (https://bit.ly/BI-Project-Slicer)

---

## ✔ Deliverables

- SQL VIEW for unified profitability analysis
- Power BI data model for interactive visuals
- KPI visual with trend analysis
- Line chart for profit over time
- Category-level profitability comparison

---

> This project provides all components you need for insightful category-level profitability analysis and reporting in Power BI!
