# Database Schema

This document describes the source e-commerce database schema, including its tables, columns, primary keys (PK), and foreign keys (FK).

- **PK** = Primary Key
- **FK** = Foreign Key

## Customers

| # | Column | Key |
|---:|---|:---:|
| 1 | CustomerID | PK |
| 2 | FullName | |
| 3 | Gender | |
| 4 | BirthDate | |
| 5 | Email | |
| 6 | Phone | |
| 7 | City | |
| 8 | State | |
| 9 | Country | |
| 10 | PostalCode | |
| 11 | RegistrationDate | |
| 12 | CustomerSegment | |
| 13 | LoyaltyLevel | |
| 14 | IsActive | |

## Orders

| # | Column | Key |
|---:|---|:---:|
| 1 | OrderID | PK |
| 2 | CustomerID | FK |
| 3 | EmployeeID | FK |
| 4 | WarehouseID | FK |
| 5 | ShippingCompanyID | FK |
| 6 | OrderDate | |
| 7 | RequiredDate | |
| 8 | ShippingDate | |
| 9 | OrderStatus | |
| 10 | PaymentMethod | |
| 11 | Currency | |

## Order Details

| # | Column | Key |
|---:|---|:---:|
| 1 | OrderDetailID | PK |
| 2 | OrderID | FK |
| 3 | ProductID | FK |
| 4 | Quantity | |
| 5 | UnitPrice | |
| 6 | DiscountPercent | |
| 7 | TaxAmount | |
| 8 | LineTotal | |

## Products

| # | Column | Key |
|---:|---|:---:|
| 1 | ProductID | PK |
| 2 | ProductName | |
| 3 | Brand | |
| 4 | Category | |
| 5 | SubCategory | |
| 6 | SupplierID | FK |
| 7 | CostPrice | |
| 8 | SellingPrice | |
| 9 | Weight | |
| 10 | Color | |
| 11 | LaunchDate | |
| 12 | Status | |

## Suppliers

| # | Column | Key |
|---:|---|:---:|
| 1 | SupplierID | PK |
| 2 | SupplierName | |
| 3 | Country | |
| 4 | LeadTimeDays | |
| 5 | SupplierRating | |
| 6 | ContactName | |

## Employees

| # | Column | Key |
|---:|---|:---:|
| 1 | EmployeeID | PK |
| 2 | EmployeeName | |
| 3 | Department | |
| 4 | JobTitle | |
| 5 | ManagerID | FK |
| 6 | HireDate | |
| 7 | Salary | |
| 8 | City | |
| 9 | Country | |

## Inventory

| # | Column | Key |
|---:|---|:---:|
| 1 | WarehouseID | PK |
| 2 | ProductID | PK |
| 3 | CurrentStock | |
| 4 | ReservedStock | |
| 5 | SafetyStock | |
| 6 | ReorderLevel | |
| 7 | LastRestocked | |

> Inventory uses a composite primary key: (WarehouseID, ProductID).

## Returns

| # | Column | Key |
|---:|---|:---:|
| 1 | ReturnID | PK |
| 2 | OrderID | FK |
| 3 | ProductID | FK |
| 4 | ReturnDate | |
| 5 | ReturnReason | |
| 6 | RefundAmount | |

## Payments

| # | Column | Key |
|---:|---|:---:|
| 1 | PaymentID | PK |
| 2 | OrderID | FK |
| 3 | PaymentMethod | |
| 4 | PaymentStatus | |
| 5 | PaymentDate | |
| 6 | AmountPaid | |

## Shipping Companies

| # | Column | Key |
|---:|---|:---:|
| 1 | ShippingCompanyID | PK |
| 2 | CompanyName | |
| 3 | AverageDeliveryDays | |
| 4 | ShippingCost | |
| 5 | TrackingSupported | |

## Warehouses

| # | Column | Key |
|---:|---|:---:|
| 1 | WarehouseID | PK |
| 2 | WarehouseName | |
| 3 | City | |
| 4 | Country | |
| 5 | Capacity | |

## Table Relationships

The following relationships are represented by the foreign-key fields in the schema:

| From | To |
|---|---|
| Orders.CustomerID | Customers.CustomerID |
| Orders.EmployeeID | Employees.EmployeeID |
| Orders.WarehouseID | Warehouses.WarehouseID |
| Orders.ShippingCompanyID | Shipping Companies.ShippingCompanyID |
| Order Details.OrderID | Orders.OrderID |
| Order Details.ProductID | Products.ProductID |
| Products.SupplierID | Suppliers.SupplierID |
| Employees.ManagerID | Employees.EmployeeID |
| Inventory.WarehouseID | Warehouses.WarehouseID |
| Inventory.ProductID | Products.ProductID |
| Returns.OrderID | Orders.OrderID |
| Returns.ProductID | Products.ProductID |
| Payments.OrderID | Orders.OrderID |
