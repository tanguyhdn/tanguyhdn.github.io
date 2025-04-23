---
author: Tanguy
categories:
- SQL
- Data Warehouse
- Analytics

date: "2025-04-10"
draft: false
excerpt: In this SQL project, I analyze retail sales data from Microsoft's ContosoRetailDW data warehouse. The focus is on store transactions, using SQL views and queries to clean, transform, and explore profitability trends across stores, products, and regions.
layout: single
links:
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/tanguyhdn/contoso-sql-retail-analytics

subtitle: Project using SQL Server & ContosoRetailDW
tags:
- SQL
- Data Analytics
- SQL Server
- Data Warehouse
- Portfolio
title: Retail Sales Analysis in SQL using ContosoRetailDW
---

In this SQL project, I analyze transactional data from Microsoft's **ContosoRetailDW** sample data warehouse. The focus is on **in-store sales only**, using the `FactSales` fact table and dimension tables for stores, geography, products, and time.



##  Data Model

The project centers around the `FactSales` fact table and its related dimensions:

- `DimDate` — calendar and date hierarchy  
- `DimProduct`, `DimProductSubcategory` — product and brand details  
- `DimStore` — physical store information  
- `DimGeography` — city, state, country for each store

![ContosoRetailDW Schema](contoso_schema.png)

## Data Cleaning View

All transformations are handled through a reusable SQL view: `vw_CleanedSales`. This view joins and enriches the raw sales data with product, store, and geography information.

```sql
CREATE OR ALTER VIEW vw_CleanedSales AS
SELECT
    fs.SalesKey,
    d.FullDateLabel AS OrderDate,
    ps.ProductSubcategoryName AS Subcategory,
    p.ProductName AS ProductName,
    p.BrandName,
    s.StoreName,
    g.CityName,
    g.StateProvinceName,
    g.RegionCountryName,
    fs.SalesAmount,
    fs.TotalCost,
    fs.SalesAmount - fs.TotalCost AS Profit,
    fs.SalesQuantity
FROM FactSales fs
JOIN DimDate d ON fs.DateKey = d.DateKey
JOIN DimProduct p ON fs.ProductKey = p.ProductKey
JOIN DimProductSubcategory ps ON p.ProductSubcategoryKey = ps.ProductSubcategoryKey
JOIN DimStore s ON fs.StoreKey = s.StoreKey
JOIN DimGeography g ON s.GeographyKey = g.GeographyKey
WHERE fs.SalesAmount > 0;
```

## Key Analytical Queries

#### Revenue by product subcategory

```SQL
SELECT
    Subcategory,
    SUM(SalesAmount) AS TotalRevenue
FROM vw_CleanedSales
GROUP BY Subcategory
ORDER BY TotalRevenue DESC;
```
#### Monthly sales trends

```SQL
SELECT
    LEFT(OrderDate, 7) AS Month,
    SUM(SalesAmount) AS MonthlyRevenue
FROM vw_CleanedSales
GROUP BY LEFT(OrderDate, 7)
ORDER BY Month;
```

#### Profitability by store

```SQL
SELECT
    StoreName,
    SUM(Profit) AS TotalProfit
FROM vw_CleanedSales
GROUP BY StoreName
ORDER BY TotalProfit DESC;
```

#### Geographic revenue insights

```SQL
SELECT
    RegionCountryName AS Country,
    StateProvinceName,
    SUM(SalesAmount) AS Revenue
FROM vw_CleanedSales
GROUP BY RegionCountryName, StateProvinceName
ORDER BY Revenue DESC;
```

#### Average order by store

```SQL
SELECT
    StoreName,
    COUNT(DISTINCT SalesKey) AS TotalOrders,
    SUM(SalesAmount) AS TotalRevenue,
    ROUND(SUM(SalesAmount) / COUNT(DISTINCT SalesKey), 2) AS AvgOrderValue
FROM vw_CleanedSales
GROUP BY StoreName
ORDER BY AvgOrderValue DESC;
```

#### Top brands by profit

```SQL
SELECT TOP 10
    BrandName,
    SUM(Profit) AS TotalProfit
FROM vw_CleanedSales
GROUP BY BrandName
ORDER BY TotalProfit DESC;
```

All SQL scripts are modular and version-controlled on GitHub. Screenshots of outputs are included to showcase query results.
