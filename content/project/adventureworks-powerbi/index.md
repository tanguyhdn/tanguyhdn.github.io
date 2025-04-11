---
author: Tanguy Hodonou
categories:
- Power BI
- Business Intelligence
- Dashboard
- Data Visualization

date: "2025-04-10"
draft: false
excerpt: This Power BI report explores financial performance using the AdventureWorks dataset. The report includes a home page and a financial overview dashboard with KPIs such as sales, profit, and margin.
layout: single
links:
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/tanguyhdn/adventureworks-powerbi/blob/main/AdventureWorks_Commercial_Overview.pbix

subtitle: Financial Overview in Power BI
tags:
- Power BI
- Sales Analysis
- Financial Dashboard
- DAX
title: Financial Overview Dashboard – AdventureWorks in Power BI
---

## 📘 Project Overview

This Power BI report provides an interactive **Financial Overview Dashboard** built on the classic **AdventureWorks** dataset.  
It is designed to simulate a real-world business intelligence use case for monitoring financial performance across time and comparing key KPIs.

The project includes:

- 🏠 **Home Page**  
  A simple landing page that introduces the report and offers intuitive navigation for users.

![Home Page](Home_page.jpg)
*The landing page provides a clean entry point to the report.*

- 💰 **Financial Overview Dashboard**  


![Financial Overview](Financial_Page.jpg)
*A dynamic financial dashboard showing profit performance and trends.*

  A comprehensive page focused on financial KPIs, including:
  - **Total Sales**
  - **Total Cost**
  - **Net Profit**
  - **Profit Margin**
---

## 📊 KPIs & DAX Highlights

- **Total Sales**  
  Based on the `LineTotal` from the `SalesOrderDetail` fact table.

- **Total Cost**  
  Calculated using `OrderQty × StandardCost`, joined from the `Product` dimension.

- **Net Profit**  
  Defined as `Total Sales - Total Cost`.

- **Profit Margin**  
  Calculated using `Net Profit / Total Sales`.

---

## 🧠 Modeling Approach

The report uses a clean **star schema** structure:

- `fSalesTransactions` as the central **fact table**
- Connected dimension tables:  
  - `dimProduct`  
  - `dimCustomer`  
  - `dimSalesPerson`  
  - `dimSalesTerritory`  
  - `dimDate` (custom calendar table)

All relationships are one-to-many, using surrogate keys for optimal performance.


![Data Model](diagram.jpg)  
*A star schema data model connecting fact and dimension tables for optimal performance.*

---

## 🛠️ Tools & Techniques

- **Power BI Desktop**
- **DAX** for KPIs and time intelligence
- **Star schema modeling**
- **Custom visuals** and **navigation buttons**
- **Bookmarks** for user interaction

---