# This file contains the full list of SQL queries used in this project, Inludes:
- What the query does
- Why it was written this way

---
### check dublicates :
```
SELECT CustomerID , count(*) AS dublicates
FROM online_sales_dataset
WHERE CustomerID IS NULL
GROUP by CustomerID
HAVING count(*) > 1
```
### Purpose: 
Identify rows with missing CustomerID values that appear more than once. These can't be used for customer segmentation.

### Why? 
CustomerID is a critical field. NULLs here would lead to inaccurate customer insights.

---

### delete all null values :

```
DELETE FROM online_sales_dataset
WHERE CustomerID IS NULL

```
### Purpose: 
Clean the dataset by removing rows where CustomerID is missing.

### Why? 
To ensure accurate customer analysis. Retaining NULLs would skew customer metrics.

---

### Best-Selling Products & Categories ( two parts )

```
SELECT StockCode, SUM(Quantity) AS TotalQuantitySold  --- Start of First Part
FROM online_sales_dataset
GROUP BY StockCode
ORDER BY TotalQuantitySold DESC
LIMIT 10                                              --- End of first part
```
```
SELECT                                                --- Start of second part
    Category, 
	COUNT(*) AS CategoryOrder,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM online_sales_dataset), 2) AS OrderRate
	
FROM online_sales_dataset
GROUP BY Category
```
1️⃣ First Part :
```
SELECT StockCode, SUM(Quantity) AS TotalQuantitySold  --- Start of First Part
FROM online_sales_dataset
GROUP BY StockCode
ORDER BY TotalQuantitySold DESC
LIMIT 10                                              --- End of first part
```
### Purpose:
Identify the 10 most sold products.

### Why?
SUM(Quantity)? It reflects product popularity regardless of price.

2️⃣ Second Part :
```
SELECT                                                --- Start of second part
    Category, 
	COUNT(*) AS CategoryOrder,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM online_sales_dataset), 2) AS OrderRate
	
FROM online_sales_dataset
GROUP BY Category
```
### Purpose:
Show the share of total orders each category has.

### Why?
ROUND and subquery? To express order rate as a percentage.

---

### Impact of Discounts on Sales
```
SELECT Category ,
    strftime('%Y', InvoiceDate) AS Year,
    SUM(CASE WHEN Discount > 0 THEN Quantity ELSE 0 END) AS DiscountQuantity,
    SUM(CASE WHEN Discount = 0 THEN Quantity ELSE 0 END) AS noDiscountQuantity,
    SUM(CASE WHEN Discount > 0 THEN Quantity * UnitPrice * (1 - Discount) ELSE 0 END) AS DiscountSales,
    SUM(CASE WHEN Discount = 0 THEN Quantity * UnitPrice ELSE 0 END) AS noDiscountSales
FROM online_sales_dataset
GROUP BY Category, Year;
```
### Purpose:
Analyze how discounts affect quantity and revenue by category.

### Why use CASE? 
Allows calculating two conditions in the same aggregation: with and without discounts.

### Why multiply by (1 - Discount)? 
To factor in the actual discounted price.

---

### Seasonal Sales Trends
```
SELECT
    strftime('%Y', InvoiceDate) AS Year,
    strftime('%m', InvoiceDate) AS Month,
    SUM(Quantity * UnitPrice) AS TotalSales
FROM online_sales_dataset
GROUP BY Year, Month
ORDER BY Month, Year;
```
### Purpose: Find
seasonal trends in sales.

### Why strftime?
Extracts year and month for grouping.

### Why order by Month then Year?
Ensures month sequence is chronological when slicing by year.

---

### High-Value Customers
```
SELECT CustomerID, SUM(Quantity * UnitPrice) AS TotalRevenue
FROM online_sales_dataset
GROUP BY CustomerID
HAVING TotalRevenue > 10000
ORDER BY TotalRevenue DESC;
```
### Purpose: 
Find VIP customers with over $10K revenue.

### Why HAVING?
Filters after GROUP BY to include only high-revenue customers.

---

### Return Rate Analysis
```
WITH Profitstat AS (
    SELECT
        strftime('%Y', InvoiceDate) AS Year,
        SUM(Quantity * UnitPrice) AS TotalRevenue,
        SUM(CASE WHEN ReturnStatus = 'Returned' THEN Quantity * UnitPrice ELSE 0 END) AS RevenueLost,
        SUM(Quantity * UnitPrice) - SUM(CASE WHEN ReturnStatus = 'Returned' THEN Quantity * UnitPrice ELSE 0 END) AS NetRevenue
    FROM online_sales_dataset
    GROUP BY Year
)
SELECT
    Year, TotalRevenue, RevenueLost, NetRevenue,
    ROUND(RevenueLost * 100.0 / TotalRevenue, 2) AS ReturnImpactPrecent
FROM Profitstat;
```
### Purpose:
Understand how much revenue was lost due to returns.

### Why use CTE? 
Improves clarity by separating revenue calculations from presentation.

### Why ROUND(... * 100.0 / ...)?
Calculates the return loss as a percentage of total revenue.

---

### Payment & Shipping Method Analysis
```
WITH Paystat AS (
    SELECT PaymentMethod, COUNT(PaymentMethod) AS NPay
    FROM online_sales_dataset
    GROUP BY PaymentMethod
),
Shipstat AS (
    SELECT ShipmentProvider, COUNT(ShipmentProvider) AS NShipping
    FROM online_sales_dataset
    GROUP BY ShipmentProvider
)
SELECT * FROM Paystat;     --- change the FROM Paystat to FROM Shipstat for shipping data
```
### Purpose: Identify preferred payment and shipping methods.

### Why separate CTEs?
You structured it to modularize logic.

---

### Warehouse & Fulfillment Performance
```
WITH WarehouseReturns AS (
    SELECT
        WarehouseLocation,
        COUNT(*) AS TotalOrders,
        SUM(CASE WHEN ReturnStatus = 'Returned' THEN 1 ELSE 0 END) AS Returns
    FROM online_sales_dataset
    GROUP BY WarehouseLocation
)
SELECT
    WarehouseLocation,
    TotalOrders,
    Returns,
    ROUND(Returns * 100.0 / TotalOrders, 2) AS ReturnRate
FROM WarehouseReturns
ORDER BY ReturnRate DESC;
```
### Purpose: 
Compare return rates across warehouses.

### Why use CTE? 
Keeps return aggregation logic separate for clarity and potential reuse.

### Why ORDER BY ReturnRate DESC? 
Surfaces the worst-performing warehouses first.
