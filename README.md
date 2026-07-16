# AnalystLab Africa Internship — Week 3 Project 2: SQL Data Analysis (Sales Dataset)

---

# Project Overview

This project demonstrates the use of SQL to explore, query, and analyze the Sample Sales Data dataset (used as the AdventureWorks Lite equivalent) using **DB Browser for SQLite**. The objective was to practice SQL querying techniques from basic retrieval through joins, subqueries, and window functions and generate meaningful business insights from a real-world sales dataset.

---

# Project Information

| Detail | Information |
|---------|-------------|
| Project | Week 3 Project 2 – SQL Data Analysis |
| Analyst | Benjamin Umanta Esther |
| Program | AnalystLab Africa Data Analytics Internship |
| Tool | DB Browser for SQLite |
| Database | sales.db |
| Database Type | SQLite |

---

# Project Objectives

- Import and structure a raw sales CSV into a queryable database.
- Understand the dataset structure.
- Retrieve records using SQL queries.
- Sort and filter data.
- Perform aggregations.
- Create a lookup table and use JOINs to combine tables.
- Apply subqueries and window functions.
- Answer business questions using SQL.
- Apply basic query optimization (indexing).

---

# Database Overview

The Sales dataset represents order-level transactions from a wholesale/retail business, containing:

- Order details (order number, quantity, price, line number, sales amount, date, status)
- Product information (product line, product code, MSRP)
- Customer information (name, contact, phone, address, city, state, country)
- Territory and deal size classification

A supporting lookup table, **territory_region**, was created to map each `TERRITORY` to a `REGION_MANAGER`, enabling JOIN practice since the source dataset is a single flat table.

View the raw dataset (sales_data_sample.csv (1))
Original source: Kaggle — Sample Sales Data
---

# Tools Used

- DB Browser for SQLite
- SQLite
- SQL

---

# SQL Queries

## Task 1: Database Setup

### Query 1 — Show All Tables

```sql
SELECT name FROM sqlite_master WHERE type='table';
```

**Purpose**

Confirm which tables exist in the database.

**Result**

Returned **1 table**: `sales`.

### Query 2 — View Table Structure

```sql
PRAGMA table_info(sales);
```

**Purpose**

Display the columns and data types in the `sales` table.

**Result**

Returned the complete schema — **25 columns**.

### Query 3 — Total Rows

```sql
SELECT COUNT(*) AS TotalRows FROM sales;
```

**Purpose**

Count the total number of order records.

**Result**

Returned **2,823 rows**.

### Query 4 — Total Columns

```sql
SELECT COUNT(*) AS TotalColumns FROM pragma_table_info('sales');
```

**Purpose**

Confirm the number of columns in the dataset.

**Result**

Returned **25 columns**.

---

## Task 2: Core SQL Queries

### Query 5 — View All Sales Records

```sql
SELECT * FROM sales;
```

**Purpose**

Retrieve every record in the table.

**Result**

Returned all **2,823 records**.

### Query 6 — Select Specific Columns (Customer & Country)

```sql
SELECT CUSTOMERNAME, COUNTRY FROM sales;
```

**Purpose**

Retrieve only customer name and country for each order.

**Result**

Returned 2,823 rows across **92 distinct customers**.

### Query 7 — Select Specific Columns (Product Line & Sales)

```sql
SELECT PRODUCTLINE, SALES FROM sales;
```

**Purpose**

Retrieve product line and sales amount for each order.

**Result**

Returned 2,823 rows.

### Query 8 — WHERE: Orders from France

```sql
SELECT * FROM sales WHERE COUNTRY='France';
```

**Purpose**

Filter orders placed from France.

**Result**

Returned **314 rows**.

### Query 9 — WHERE: Sales Above $3,000

```sql
SELECT PRODUCTLINE, SALES FROM sales WHERE SALES>3000;
```

**Purpose**

Identify higher-value individual orders.

**Result**

Returned **1,541 rows**.

### Query 10 — WHERE: Orders That Are Shipped

```sql
SELECT ORDERNUMBER, STATUS FROM sales WHERE STATUS='Shipped';
```

**Purpose**

Filter orders with a "Shipped" status.

**Result**

Returned **2,617 rows**.

### Query 11 — WHERE: Small Deal Size Orders

```sql
SELECT ORDERNUMBER, DEALSIZE FROM sales WHERE DEALSIZE='Small';
```

**Purpose**

Filter orders classified as small deals.

**Result**

Returned **1,282 rows**.

### Query 12 — ORDER BY: Products by Sales Descending

```sql
SELECT PRODUCTLINE, SALES FROM sales ORDER BY SALES DESC;
```

**Purpose**

Rank individual orders from highest to lowest sales value.

**Result**

Highest single order: **Vintage Cars, $14,082.80**.

### Query 13 — ORDER BY: Orders by Date

```sql
SELECT ORDERNUMBER, ORDERDATE FROM sales ORDER BY ORDERDATE;
```

**Purpose**

Sort orders chronologically.

**Result**

Earliest order: **#10102, 1/10/2003**.

### Query 14 — ORDER BY: Customers Alphabetically

```sql
SELECT DISTINCT CUSTOMERNAME FROM sales ORDER BY CUSTOMERNAME;
```

**Purpose**

List unique customers in alphabetical order.

**Result**

Returned **92 distinct customers**.

### Query 15 — ORDER BY: Highest Quantity Ordered

```sql
SELECT ORDERNUMBER, QUANTITYORDERED FROM sales ORDER BY QUANTITYORDERED DESC;
```

**Purpose**

Identify orders with the largest quantities.

**Result**

Highest quantity: **Order #10405, 97 units**.

### Query 16 — LIMIT: Top 5 Highest Sales

```sql
SELECT ORDERNUMBER, SALES FROM sales ORDER BY SALES DESC LIMIT 5;
```

**Purpose**

Retrieve the 5 highest-value orders.

**Result**

Top order: **#10407, $14,082.80**, followed by #10322 ($12,536.50), #10424 ($12,001.00), #10412 ($11,887.80), #10403 ($11,886.60).

### Query 17 — LIMIT: First 10 Orders

```sql
SELECT * FROM sales LIMIT 10;
```

**Purpose**

Preview the first 10 records in the table.

**Result**

Returned **10 rows**.

### Query 18 — LIMIT: Top 5 Largest Quantities

```sql
SELECT ORDERNUMBER, QUANTITYORDERED FROM sales ORDER BY QUANTITYORDERED DESC LIMIT 5;
```

**Purpose**

Identify the 5 largest-quantity orders.

**Result**

Top order: **#10405, 97 units**.

### Query 19 — LIMIT: 5 Most Recent Orders

```sql
SELECT ORDERNUMBER, ORDERDATE FROM sales ORDER BY ORDERDATE DESC LIMIT 5;
```

**Purpose**

Retrieve the most recent order dates.

**Result**

Returned the 5 latest orders in the dataset.

### Query 20 — Aggregate: Number of Orders

```sql
SELECT COUNT(*) AS TotalOrders FROM sales;
```

**Purpose**

Count the total number of order line records.

**Result**

Returned **2,823 orders**.

### Query 21 — Aggregate: Total Revenue

```sql
SELECT SUM(SALES) AS TotalRevenue FROM sales;
```

**Purpose**

Calculate total revenue across all orders.

**Result**

Returned **$10,032,628.85**.

### Query 22 — Aggregate: Average Order Value

```sql
SELECT AVG(SALES) AS AverageOrderValue FROM sales;
```

**Purpose**

Calculate the average value per order.

**Result**

Returned **$3,553.89**.

### Query 23 — Aggregate: Highest Single Sale

```sql
SELECT MAX(SALES) AS HighestSale FROM sales;
```

**Purpose**

Identify the highest individual sale.

**Result**

Returned **$14,082.80**.

### Query 24 — Aggregate: Lowest Single Sale

```sql
SELECT MIN(SALES) AS LowestSale FROM sales;
```

**Purpose**

Identify the lowest individual sale.

**Result**

Returned **$482.13**.

### Query 25 — Aggregate: Total Units Sold

```sql
SELECT SUM(QUANTITYORDERED) AS TotalUnitsSold FROM sales;
```

**Purpose**

Calculate the total number of items sold.

**Result**

Returned **99,067 units**.

---

## Task 3: Advanced SQL Concepts

### Query 26 — Create Lookup Table for Join Practice

```sql
CREATE TABLE territory_region (
    TERRITORY TEXT PRIMARY KEY,
    REGION_MANAGER TEXT
);

INSERT INTO territory_region (TERRITORY, REGION_MANAGER) VALUES
('EMEA', 'Amara Okafor'),
('APAC', 'David Chen'),
('Japan', 'Yuki Tanaka'),
('NA', 'Sarah Mitchell');
```

**Purpose**

Since the source dataset is a single flat table, a lookup table was created mapping each territory to a region manager, enabling meaningful JOIN practice.

**Result**

Created successfully — 4 territory/manager pairs.

### Query 27 — INNER JOIN: Sales with Region Manager

```sql
SELECT s.ORDERNUMBER, s.TERRITORY, s.SALES, t.REGION_MANAGER
FROM sales s
INNER JOIN territory_region t ON s.TERRITORY = t.TERRITORY;
```

**Purpose**

Match each sales record to its region manager.

**Result**

Returned matched records for all territories present in both tables.

### Query 28 — INNER JOIN: Total Sales per Manager

```sql
SELECT t.REGION_MANAGER, SUM(s.SALES) AS TotalSales
FROM sales s
INNER JOIN territory_region t ON s.TERRITORY = t.TERRITORY
GROUP BY t.REGION_MANAGER
ORDER BY TotalSales DESC;
```

**Purpose**

Determine total revenue managed by each region manager.

**Result**

Amara Okafor (EMEA) led, followed by Sarah Mitchell (NA), David Chen (APAC), and Yuki Tanaka (Japan).

### Query 29 — LEFT JOIN: All Sales, Manager or NULL

```sql
SELECT s.ORDERNUMBER, s.TERRITORY, s.SALES, t.REGION_MANAGER
FROM sales s
LEFT JOIN territory_region t ON s.TERRITORY = t.TERRITORY;
```

**Purpose**

Retain every sales record even if no matching territory exists in the lookup table.

**Result**

Returned all 2,823 rows, each with a matched manager (no unmatched territories found).

### Query 30 — LEFT JOIN: Territories With No Manager Match

```sql
SELECT DISTINCT s.TERRITORY
FROM sales s
LEFT JOIN territory_region t ON s.TERRITORY = t.TERRITORY
WHERE t.REGION_MANAGER IS NULL;
```

**Purpose**

Check for any territory values in `sales` that don't exist in the lookup table.

**Result**

Returned **0 rows** — every territory value matched successfully.

### Query 31 — RIGHT JOIN: All Managers With Sales or NULL

```sql
SELECT t.TERRITORY, t.REGION_MANAGER, s.ORDERNUMBER, s.SALES
FROM sales s
RIGHT JOIN territory_region t ON s.TERRITORY = t.TERRITORY;
```

**Purpose**

Ensure every manager appears even if they had zero matching sales.

**Result**

Returned all managers with their matched sales records.

### Query 32 — RIGHT JOIN: Order Count per Manager

```sql
SELECT t.REGION_MANAGER, COUNT(s.ORDERNUMBER) AS OrderCount
FROM sales s
RIGHT JOIN territory_region t ON s.TERRITORY = t.TERRITORY
GROUP BY t.REGION_MANAGER;
```

**Purpose**

Count orders handled per manager, including any with zero.

**Result**

Amara Okafor: 1,407 orders; Sarah Mitchell: 1,074; David Chen: 221; Yuki Tanaka: 121.

### Query 33 — Subquery: Orders Above Average Sales

```sql
SELECT ORDERNUMBER, SALES FROM sales
WHERE SALES > (SELECT AVG(SALES) FROM sales);
```

**Purpose**

Identify above-average value orders using a nested query.

**Result**

Returned orders exceeding the average order value of $3,553.89.

### Query 34 — Subquery: Customers With Above-Average Orders

```sql
SELECT DISTINCT CUSTOMERNAME FROM sales
WHERE ORDERNUMBER IN (
    SELECT ORDERNUMBER FROM sales WHERE SALES > (SELECT AVG(SALES) FROM sales)
);
```

**Purpose**

Identify which customers placed at least one above-average order.

**Result**

Returned the subset of customers meeting this condition.

### Query 35 — Subquery: Product Lines Above Average Line Total

```sql
SELECT PRODUCTLINE, SUM(SALES) AS LineTotal FROM sales
GROUP BY PRODUCTLINE
HAVING SUM(SALES) > (
    SELECT AVG(LineTotal) FROM (
        SELECT SUM(SALES) AS LineTotal FROM sales GROUP BY PRODUCTLINE
    )
);
```

**Purpose**

Identify product lines performing above the average product line total.

**Result**

Classic Cars and Vintage Cars were the standout lines exceeding the average.

### Query 36 — Subquery: Territory With the Highest Single Order

```sql
SELECT TERRITORY, ORDERNUMBER, SALES FROM sales
WHERE SALES = (SELECT MAX(SALES) FROM sales);
```

**Purpose**

Find the territory associated with the single highest sale.

**Result**

Returned **NA, Order #10407, $14,082.80**.

### Query 37 — Window Function: Row Number by Sales

```sql
SELECT ORDERNUMBER, SALES,
ROW_NUMBER() OVER (ORDER BY SALES DESC) AS RowNum
FROM sales;
```

**Purpose**

Assign a sequential rank to every order by sales value.

**Result**

Returned all 2,823 orders numbered from highest to lowest sale.

### Query 38 — Window Function: Rank by Territory

```sql
SELECT TERRITORY, ORDERNUMBER, SALES,
RANK() OVER (PARTITION BY TERRITORY ORDER BY SALES DESC) AS TerritoryRank
FROM sales;
```

**Purpose**

Rank orders by sales value within each territory.

**Result**

Returned each territory's orders ranked independently.

### Query 39 — Window Function: Rank Product Lines by Revenue

```sql
SELECT PRODUCTLINE, SUM(SALES) AS LineTotal,
RANK() OVER (ORDER BY SUM(SALES) DESC) AS LineRank
FROM sales
GROUP BY PRODUCTLINE;
```

**Purpose**

Rank all 7 product lines by total revenue.

**Result**

Classic Cars ranked #1 ($3,919,615.66).

### Query 40 — Window Function: Top 3 Orders per Customer

```sql
SELECT * FROM (
    SELECT CUSTOMERNAME, ORDERNUMBER, SALES,
    ROW_NUMBER() OVER (PARTITION BY CUSTOMERNAME ORDER BY SALES DESC) AS CustomerRank
    FROM sales
) WHERE CustomerRank <= 3;
```

**Purpose**

Identify each customer's 3 highest-value orders.

**Result**

Returned **276 rows** (top 3 per customer across 92 customers).

---

## Task 4: Business Problem Solving

### Query 41 — Top 5 Customers by Total Spending

```sql
SELECT CUSTOMERNAME, SUM(SALES) AS TotalSpent
FROM sales
GROUP BY CUSTOMERNAME
ORDER BY TotalSpent DESC
LIMIT 5;
```

**Result**

Euro Shopping Channel ($912,294.11), Mini Gifts Distributors Ltd. ($654,858.06), Australian Collectors, Co. ($200,995.41), Muscle Machine Inc ($197,736.94), La Rochelle Gifts ($180,124.90).

### Query 42 — Top-Performing Product Line by Revenue

```sql
SELECT PRODUCTLINE, SUM(SALES) AS TotalRevenue
FROM sales
GROUP BY PRODUCTLINE
ORDER BY TotalRevenue DESC;
```

**Result**

Classic Cars led with **$3,919,615.66**, followed by Vintage Cars ($1,903,150.84) and Motorcycles ($1,166,388.34).

### Query 43 — Revenue Trend by Year

```sql
SELECT YEAR_ID, SUM(SALES) AS YearlyRevenue
FROM sales
GROUP BY YEAR_ID
ORDER BY YEAR_ID;
```

**Result**

2003: $3,516,979.54; 2004: $4,724,162.60; 2005: $1,791,486.71 (partial year).

### Query 44 — Revenue Trend by Month

```sql
SELECT MONTH_ID, SUM(SALES) AS MonthlyRevenue
FROM sales
GROUP BY MONTH_ID
ORDER BY MONTH_ID;
```

**Result**

November was the strongest month by far (**$2,118,885.67**), more than double most other months.

### Query 45 — Average Order Value by Deal Size

```sql
SELECT DEALSIZE, COUNT(*) AS OrderCount, AVG(SALES) AS AvgOrderValue
FROM sales
GROUP BY DEALSIZE
ORDER BY AvgOrderValue DESC;
```

**Result**

Large: 157 orders, avg $8,293.75; Medium: 1,384 orders, avg $4,398.43; Small: 1,282 orders, avg $2,061.68.

---

## Task 5: Query Optimization

### Query 46–48 — Create Indexes

```sql
CREATE INDEX idx_customername ON sales(CUSTOMERNAME);
CREATE INDEX idx_territory ON sales(TERRITORY);
CREATE INDEX idx_orderdate ON sales(ORDERDATE);
```

**Purpose**

Speed up frequent filter/join operations on customer, territory, and date fields.

**Result**

3 indexes created successfully.

### Query 49 — Verify Indexes

```sql
SELECT name FROM sqlite_master WHERE type='index';
```

**Result**

Confirmed all 3 indexes exist in the database.

---

# Business Questions

### Who are the top customers?

**Insight**

Euro Shopping Channel and Mini Gifts Distributors Ltd. together account for a disproportionate share of revenue a concentrated customer base worth prioritizing for retention.

### Which product line performs best?

**Insight**

Classic Cars dominates revenue, generating over double the next closest line, showing heavy reliance on a single category.

### How does revenue trend over time?

**Insight**

Revenue grew ~34% from 2003 to 2004; the 2005 figure reflects a partial year rather than a decline. November stands out as a strong seasonal peak.

### How do customers behave by deal size?

**Insight**

Large deals are rare (157 orders) but roughly 4x more valuable on average than Small deals, meaning a small number of large transactions carry outsized weight.

---

# Key Findings

- The dataset contains **1 table** (`sales`) with **25 columns** and **2,823 rows**.
- A supporting `territory_region` lookup table was created to enable JOIN practice.
- Total revenue across all orders: **$10,032,628.85**.
- Average order value: **$3,553.89**.
- Top customer: **Euro Shopping Channel** ($912,294.11).
- Top product line: **Classic Cars** ($3,919,615.66).
- Strongest month: **November** ($2,118,885.67).
- SQL JOINs, subqueries, and window functions all successfully applied to a single flat table by introducing a lookup table.

---

# Challenges Encountered

- The dataset is a single flat table, unlike Chinook's multiple related tables a `territory_region` lookup table had to be created from scratch to practice joins meaningfully.
- The CSV file used non-standard text encoding, which caused import errors until it was changed to ISO-8859-1.
- The imported table name didn't match the script (imported as `sale` instead of `sales`), requiring an `ALTER TABLE RENAME` to fix.
- Losing unsaved script progress after a session reset, reinforcing the need to save the SQL project file regularly.
- Writing correct window function syntax (`PARTITION BY` combined with `ROW_NUMBER`/`RANK`) for per-group ranking.

---

# Recommendations

- Strengthen relationships with top accounts like Euro Shopping Channel through dedicated account management.
- Continue investing in Classic Cars while exploring growth in underperforming lines such as Trains and Ships.
- Plan inventory and marketing campaigns around the strong November sales peak.
- Develop strategies to convert more Medium/Small deals into Large deals, given their higher average value.
- Use SQL-based reporting regularly to monitor customer concentration and seasonal trends.

---

# Repository Structure

```
Week3_Project2_SQL/
│
├── README.md
├── sales_data_sample.csv
```

---

# About

**Analyst:** Benjamin Umanta Esther 

**Program:** AnalystLab Africa Data Analytics Internship

**Project:** Week 3 Project 2 — SQL Data Analysis using the Sales Dataset

This project demonstrates practical SQL skills including querying, filtering, sorting, aggregation, joins, subqueries, window functions, indexing, and business insight generation using a relational sales database.
