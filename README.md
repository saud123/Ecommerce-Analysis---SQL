<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*RN0VxTZiCzKTQbuwmOSYzA.jpeg" alt="SQL Data Analyst Journey" width="900" height="450">


# SQL Queries Documentation – Ecommerce Analysis

This repository contains SQL queries for analyzing an Ecommerce dataset. Each query includes its **purpose, focus, SQL code, expected output, and business insight**.

## SQL Queries

## Query 1: Select All Columns
 Purpose: Get a full view of the customer table.
 Focus: SELECT *
 Expected Output: All columns and rows.
 Business Insight: Provides an overview of available data fields.

``` SELECT * FROM [EcommmerceDB].[dbo].[Customer]; ```

## Query 2: Select Specific Columns
 Purpose: Focus on key customer information (name, country, city).
 Focus: SELECT, AS
 Expected Output: Customer_Name, Country, City columns only.
 Business Insight: Highlights customer demographics.

``` SELECT Customer_Name AS Full_Name, Country, City FROM [EcommerceDB].[dbo].[Customer]; ```

## Query 3: WHERE Filtering
Purpose: Find all customers located in Madison.
Focus: WHERE
Expected Output: List of customers in Madison.
Business Insight: Identifies regional customer base.

``` SELECT Customer_Name, Country, City FROM [EcommerceDB].[dbo].[Customer]
WHERE City = 'Madison';
```
## Query 4: Multiple Conditions
Purpose: Identify U.S. consumers with sales > $100.
Focus: WHERE, AND
Expected Output: Customers meeting criteria.
Business Insight: Pinpoints valuable customers for marketing.

```
SELECT *
FROM [EcommerceDB].[dbo].[Customer]
WHERE Segment = 'Consumer' AND Country = 'United States' AND Sales > 100;
```
## Query 5: Sort Customers by Registration Date (Newest First)
Purpose: Identify newest customers.
Focus: ORDER BY, DESC
Expected Output: Customers sorted from newest to oldest.
Business Insight: Helps target recent joiners.
```
SELECT Customer_Name, Country, City, Registration_Date
FROM [EcommerceDB].[dbo].[Customer]
ORDER BY Registration_Date DESC;
```
## Query 6: Count Customers by City
Purpose: Determine cities with highest customer counts.
Focus: COUNT, GROUP BY, ORDER BY
Expected Output: Number of customers per city.
Business Insight: Reveals largest customer bases geographically.
```
SELECT COUNT(*) AS Count_Customer, City
FROM [EcommerceDB].[dbo].[Customer]
GROUP BY City
ORDER BY Count_Customer DESC;
```
## Query 7: SUM by Product Category
Purpose: Find best-selling product categories.
Focus: SUM, JOIN, GROUP BY, ORDER BY
Expected Output: Total sales per product category.
Business Insight: Technology products dominated sales.
```
SELECT P.Category, SUM(S.Sales) AS TotalSales
FROM [EcommerceDB].[dbo].[Customer] S
JOIN [EcommerceDB].[dbo].[Product] P ON S.Product_ID = P.Product_ID
GROUP BY P.Category
ORDER BY TotalSales DESC;
```
## Query 8: Average Order Value by Customer Segment
Purpose: Find segments generating highest revenue.
Focus: AVG, JOIN, GROUP BY
Expected Output: Average order value per segment.
Business Insight: Home Office segment leads; prioritize investment.
```
SELECT S.Segment, AVG(S.Sales) AS Avg_Order_Value
FROM [EcommerceDB].[dbo].[Customer] S
JOIN [EcommerceDB].[dbo].[Product] P ON S.Product_ID = P.Product_ID
GROUP BY S.Segment
ORDER BY Avg_Order_Value DESC;
```
## Query 9: Cities with More Than 10 Customers
Purpose: Identify cities with significant customer base.
Focus: COUNT, GROUP BY, HAVING, ORDER BY
Expected Output: Cities with more than 10 customers.
Business Insight: Key markets for focused campaigns.
```
SELECT Count(Customer_ID) AS Count_Customer, City
FROM [EcommerceDB].[dbo].[Customer]
GROUP BY City
HAVING Count(Customer_ID) > 10
ORDER BY Count_Customer DESC;
```
## Query 10: INNER JOIN Customers + Orders
Purpose: Retrieve customer details with orders.
Focus: INNER JOIN, table aliasing
Expected Output: Active customers with order info.
Business Insight: Focus on active order patterns.
```
SELECT
C.Customer_Name,
C.City,
C.Country,
O.Order_Date,
O.Ship_Mode
FROM EcommerceDB.dbo.Customer AS C
INNER JOIN EcommerceDB.dbo.[Order] AS O
ON C.Customer_ID = O.Customer_ID;
```
## Query 11: LEFT JOIN Customers without Orders
Purpose: Include all customers.
Focus: LEFT JOIN
Expected Output: Customers including those without orders.
Business Insight: Identifies inactive or potential customers.
```
SELECT
C.Customer_Name,
C.Country,
O.Order_Date,
O.Ship_Mode
FROM EcommerceDB.dbo.Customer AS C
LEFT JOIN EcommerceDB.dbo.[Order] AS O
ON C.Customer_ID = O.Customer_ID;
```
## Query 12: RIGHT JOIN Products without Sales
Purpose: Show all products including unsold.
Focus: RIGHT JOIN
Expected Output: Products with sales info.
Business Insight: Inventory review, underperforming products.
```
SELECT
P.Category,
P.Sub_Category,
P.Product_Name,
C.Sales
FROM EcommerceDB.dbo.Customer AS C
RIGHT JOIN EcommerceDB.dbo.Product AS P
ON C.Product_ID = P.Product_ID;
```
## Query 13: Multiple JOINs (Customer + Order + Product)
Purpose: Combine customer, order, and product data.
Focus: INNER JOIN, table aliasing, column aliasing
Expected Output: Consolidated dataset.
Business Insight: Holistic view for reporting.
```
SELECT
C.Customer_Name,
C.Country AS Customer_Country,
C.City AS Customer_City,
O.Ship_Mode,
O.Order_Date,
P.Category AS Product_Category,
P.Sub_Category AS Product_Sub_Category,
P.Product_Name AS Product_Name
FROM [EcommerceDB].[dbo].[Customer] C
INNER JOIN [EcommerceDB].[dbo].[Order] O
ON C.Order_ID = O.Order_ID
INNER JOIN [EcommerceDB].[dbo].[Product] P
ON C.Product_ID = P.Product_ID;
```
## Query 14: CTE for Monthly High/Low Sales
Purpose: Identify monthly sales peaks and dips.
Focus: CTE, MAX, MIN, GROUP BY
Expected Output: Monthly highest and lowest sales values.
Business Insight: Highlights seasonal trends and performance peaks/dips.
```
WITH CustomerOrders AS (
SELECT C.Customer_ID, C.Customer_Name, C.Sales, O.Order_Date,
DATENAME(MONTH, O.Order_Date) AS OrderMonth
FROM Customer C
JOIN [Order] O ON C.Customer_ID = O.Customer_ID
)
SELECT OrderMonth, MAX(Sales) AS Highest_Value, MIN(Sales) AS Lowest_Value
FROM CustomerOrders
GROUP BY OrderMonth;
```
## Query 15: Monthly Sales Report
Purpose: Build business-ready monthly sales report.
Focus: JOIN, GROUP BY, ORDER BY, Date formatting
Expected Output: Monthly sales per segment and product.
Business Insight: Identifies peak sales periods, top products, and high-performing segments.
```
SELECT
C.Segment AS customer_segment,
SUM(C.Sales) AS sales,
YEAR(O.Order_Date) AS Order_Year,
MONTH(O.Order_Date) AS OrderMonthNumber,
DATENAME(MONTH, O.Order_Date) AS OrderMonth,
P.Product_Name AS Products
FROM [EcommerceDB].[dbo].[Customer] C
INNER JOIN [EcommerceDB].[dbo].[Order] O
ON C.Order_ID = O.Order_ID
INNER JOIN [EcommerceDB].[dbo].[Product] P
ON C.Product_ID = P.Product_ID
GROUP BY
C.Segment,
YEAR(O.Order_Date),
MONTH(O.Order_Date),
DATENAME(MONTH, O.Order_Date),
P.Product_Name
ORDER BY
Order_Year DESC,
OrderMonthNumber DESC,
Sales DESC;
```

## Conclusion

In this mini SQL project, I explored an **Ecommerce Sales Dataset** and built a series of queries ranging from basic `SELECT` statements to complex multi-table `JOINs` and `CTEs`.

**Key takeaways:**

- SQL transforms business questions into actionable insights.
- Aggregation functions (`SUM`, `COUNT`, `AVG`) reveal patterns in customer behavior and product performance.
- Multi-table operations (`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`) consolidate datasets for holistic analysis.
- CTEs and advanced queries simplify complex reporting and help identify trends such as monthly sales peaks and dips.
- Consistent practice and hands-on learning are crucial to mastering SQL for real-world data analysis.

This journey demonstrated how a **structured approach**—from simple queries to advanced reporting—can empower a data analyst to derive meaningful insights and support data-driven decisions.
