E-commerce Data Warehouse | SQL Server, SSIS & Power BI
---

An end-to-end analytics project for a fictional e-commerce company : ten years of **synthetic** sales, customer, product, inventory and returns data cleaned in Power Query, stored in SQL Server, loaded into a dimensional data warehouse with **SSIS**, and reported in a three-page **Power BI** dashboard.
---

## Project Objectives

- Organise e-commerce data into fact and dimension tables (dimensional modelling).
- Build an SSIS ETL workflow that resolves dimension keys with Lookup transformations.
- Use SQL Server to store, query and validate the data.
- Analyse sales, profit, customers, inventory and returns in Power BI with DAX measures.


## Data Warehouse Design

The warehouse has **8 dimension tables and 3 fact tables**.

### Dimension tables

| Table | Description |
|---|---|
| `DimCustomer` | Customer name, city, state, country, gender and segment |
| `DimDate` | Calendar attributes for time analysis (`DateNum` links to the facts) |
| `DimEmployee` | Employees handling orders |
| `DimPayment` | Payment method, status and amount |
| `DimProduct` | Product, brand, category, subcategory, cost and selling price (1,980 products) |
| `DimShippingCompany` | Shipping carriers |
| `DimSupplier` | Supplier name, country, lead time and rating (70 suppliers) |
| `DimWarehouse` | Warehouse name, city, country and capacity (20 warehouses) |

### Fact tables

| Table | Description | Size |
|---|---|---|
| `FactSales` | Sales records at order-line level (includes `OrderDetailID`) with quantity, price, discount, tax and line total | 404,121 rows |
| `FactReturns` | One row per return (`ReturnID`), with return reason and refund amount | 14,715 rows |
| `FactInventory` | Stock by warehouse and product: current, reserved and safety stock, reorder level | 39,402 rows, 1,980 products |

Each dimension carries a numeric key (for example `ProductNum`, `CustomerNum`) next to the source identifier (`ProductID`, `CustomerID`).

### Relationship validation

A SQL check of orphan foreign-key values across **13 fact-to-dimension relationships** returned `OrphanRows = 0` for every relationship, and candidate key columns were unique and non-null. This validates the key values; it does not by itself prove that SQL Server `FOREIGN KEY` constraints are defined.

---

## ETL (SSIS)

The package shown above uses an **OLE DB Source**, a chain of nine **Lookup** transformations connected through *Lookup Match Output*, and an **OLE DB Destination**. Lookups match incoming rows against dimension tables to retrieve the dimension keys stored in the fact table.


## SQL Analysis and Validation

SQL Server was used to explore the warehouse, validate it and investigate data quality.

- **Aggregations:** `SUM()`, `COUNT()`, `AVG()`, `GROUP BY`, `HAVING`
- **Joins:** combining facts and dimensions, finding unmatched records
- **Subqueries:** nested analysis and comparisons
- **Window functions:** `ROW_NUMBER()` and `RANK()` for ranking and de-duplication
- **Validation:** orphan foreign keys, duplicate keys, distinct orders, products below reorder level, product names shared by several products

---

## Power BI Report

### Pages

| Page | Content |
|---|---|
| **Sales Overview** | KPI cards (Total Sales, Total Orders, Total Cost, Total Profit, Profit Margin); sales vs. profit by category; sales by country; profit by year; brand contribution. Slicers: Quarter, Year |
| **Products & Customers Analysis** | KPI cards (customers, countries, products, brands, categories); customers by country; top 5 products by profit; profit by gender and by customer segment. Slicers: Segmentation, Category |
| **Inventory & Returns Analysis** | KPI cards (warehouses, suppliers, returns, stock quantity, products below reorder level); refund amount by return reason and by year; reorder exposure by warehouse; reserved stock by warehouse |

All pages have navigation buttons and a Clear Filters button.

---

## Key Insights

Findings read from the dashboard (descriptive; they describe the simulated dataset):

1. **Top-selling country:** Saudi Arabia recorded the highest sales over the ten-year period among the top five countries displayed, narrowly ahead of Canada and Australia.
2. **Best-selling category:** Electronics was the best-selling product category, ahead of Fashion.
3. **Customer segments:** Regular customers contributed the most profit of any segment, well above VIP.
4. **Returns:** Defective Item recorded the highest refund amount (about 4.0M), followed by Item Damaged in Transit (about 3.9M). The chart measures refund amount, not the share of returns.

---


