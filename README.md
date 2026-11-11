# Duka Retail Sales Analysis

## Project Overview

Duka Retail Sales Analysis is a PostgreSQL project that explores
customer and order data for duka smart electronics. It is designed to
demonstrate how SQL can be used to organize transactional data, answer
business questions, evaluate sales performance, and produce insights
that can support management decisions.

The project focuses on customer profiles, orders,quantity, products, payment
methods, revenue, costs, profit, and sales trends.

## Project Objectives

-   Create and populate a relational database for retail customers and
    orders.
-   Explore customer demographics, locations, registration dates, and
    customer segments.
-   Measure order activity, sales revenue, and average order value.
-   Compare product and product-category performance.
-   Calculate costs, profit, and profit margins.
-   Examine monthly revenue patterns and changes over time.
-   Identify high-value customers and customer purchasing behaviour.
-   Translate query results into practical business recommendations.

## Tools and Technologies

  -----------------------------------------------------------------------
  Tool / Technology                   Purpose
  ----------------------------------- -----------------------------------
  PostgreSQL                          Relational database management
                                      system used to create, store, and
                                      query the data

  SQL                                 Data definition, data loading,
                                      filtering, aggregation, joins, and
                                      analysis

  PL/pgSQL                            Implementation of a customer
                                      revenue function

  DBeaver (optional client)           Can be used to connect to
                                      PostgreSQL and run the SQL script
  -----------------------------------------------------------------------

The SQL syntax in the project is PostgreSQL-specific in places,
including `DATE_TRUNC`, `FILTER`, `NTILE`, `LAG`,
`CREATE OR REPLACE FUNCTION`, and PL/pgSQL.

## Database Structure

The script creates a schema named `retail` and defines two tables:
`customers` and `orders`.

### 1. `customers`

Stores customer profile information.

  -----------------------------------------------------------------------
  Column                  Data type               Description
  ----------------------- ----------------------- -----------------------
  `customer_id`           `INTEGER`               Primary key identifying
                                                  each customer

  `customer_name`         `VARCHAR(100)`          Customer's name;
                                                  required

  `gender`                `VARCHAR(10)`           Gender value
                                                  constrained to `Male`
                                                  or `Female`

  `city`                  `VARCHAR(50)`           Customer's city

  `registration_date`     `DATE`                  Date the customer
                                                  registered

  `customer_segment`      `VARCHAR(20)`           Customer segment
                                                  constrained to
                                                  `Retail`, `Corporate`,
                                                  or `VIP`
  -----------------------------------------------------------------------

### 2. `orders`

Stores order-level transaction information.

  -------------------------------------------------------------------------
  Column                  Data type               Description
  ----------------------- ----------------------- -------------------------
  `order_id`              `INTEGER`               Primary key identifying
                                                  each order

  `customer_id`           `INTEGER`               Foreign key referencing
                                                  `customers.customer_id`

  `order_date`            `DATE`                  Date the order was placed

  `product_name`          `VARCHAR(100)`          Product ordered

  `product_category`      `VARCHAR(50)`           Product category

  `quantity`              `INTEGER`               Number of units ordered;
                                                  must be greater than zero

  `unit_price`            `NUMERIC(12,2)`         Selling price per unit;
                                                  must be non-negative

  `cost_price`            `NUMERIC(12,2)`         Cost price per unit; must
                                                  be non-negative

  `discount`              `NUMERIC(5,2)`          Discount percentage;
                                                  defaults to zero and is
                                                  constrained between 0 and
                                                  100

  `payment_method`        `VARCHAR(30)`           Payment method
                                                  constrained to M-Pesa,
                                                  Cash, Card, or Bank
                                                  Transfer

  `order_status`          `VARCHAR(20)`           Order status constrained
                                                  to Completed, Pending, or
                                                  Cancelled
  -------------------------------------------------------------------------

### Table Relationship

One customer can be associated with multiple orders. Each order
references one customer through `orders.customer_id`.

``` mermaid
erDiagram
    CUSTOMERS ||--o{ ORDERS : places
    CUSTOMERS {
        INTEGER customer_id PK
        VARCHAR customer_name
        VARCHAR gender
        VARCHAR city
        DATE registration_date
        VARCHAR customer_segment
    }
    ORDERS {
        INTEGER order_id PK
        INTEGER customer_id FK
        DATE order_date
        VARCHAR product_name
        VARCHAR product_category
        INTEGER quantity
        NUMERIC unit_price
        NUMERIC cost_price
        NUMERIC discount
        VARCHAR payment_method
        VARCHAR order_status
    }
```

## Dataset Description

The SQL script includes insert statements for:

-   **150 customer records** in the `customers` table.
-   **800 order records** in the `orders` table, with order IDs running
    from 1001 to 1800.
-   Order dates spanning **1 January 2025 to 30 September 2026**.
-   Customer locations including Nairobi, Thika, Meru, Nyeri, Nakuru,
    Mombasa, Kisumu, Eldoret, Kiambu, and Machakos.
-   Customer segments: Retail, Corporate, and VIP.
-   Order statuses: Completed, Pending, and Cancelled.
-   Payment methods: M-Pesa, Cash, Card, and Bank Transfer.

The products cover categories such as Electronics, Accessories,
Computing, Home Appliances, and Mobile Phones. Prices are represented in
the script as KSh values in the project comments and insights.

## Data Integrity and Performance

The database uses several constraints to protect data consistency:

-   Primary keys identify customers and orders.
-   A foreign key ensures that an order references an existing customer.
-   `NOT NULL` is applied to required customer and order fields.
-   `CHECK` constraints restrict selected values and numeric ranges.
-   `ON CONFLICT ... DO NOTHING` is used when inserting customer and
    order records, preventing duplicate primary-key inserts from
    interrupting a rerun.

The script also creates two indexes:

``` sql
CREATE INDEX IF NOT EXISTS idx_orders_customer ON orders (customer_id);
CREATE INDEX IF NOT EXISTS idx_orders_date ON orders (order_date);
```

These indexes are intended to support queries that filter or join orders
by customer and date.

## Analysis Performed

The script contains 46 numbered SQL exercises and supporting setup,
data-loading, and validation statements. The analysis progresses from
basic SQL operations to more advanced analytical techniques.

### 1. SQL Fundamentals

The project retrieves customer and order records and applies:

-   `SELECT` and column selection.
-   `WHERE` filters.
-   `DISTINCT` for unique city values.
-   `LIKE` for name-pattern searches.
-   `BETWEEN`-style range logic.
-   `ORDER BY` and `LIMIT`.
-   Aggregate functions such as `COUNT`, `SUM`, `AVG`, `MIN`, and `MAX`.

### 2. Aggregations and Grouping

The queries calculate:

-   Total customers and orders.
-   Completed-order revenue and average completed-order value.
-   Revenue by product category.
-   Customer counts by city.
-   Quantity ordered by product.
-   Cancelled-order percentage.
-   Categories whose revenue exceeds a specified threshold.

### 3. Joins and Customer Analysis

The project joins customer and order records to investigate:

-   Orders alongside customer names and cities.
-   Total spending by customer.
-   Customers who have never ordered.
-   The highest-spending customers.
-   Revenue by customer city.
-   Customers with more than five orders.
-   Average order-level spending by customer segment.
-   Popular product categories among VIP customers.
-   Customers purchasing across at least three product categories.

### 4. Subqueries and Common Table Expressions

Common Table Expressions (CTEs) and subqueries are used to make
multi-step analysis easier to follow. Examples include:

-   Customers spending above an average-spending benchmark.
-   The three highest-revenue months.
-   Category-level revenue, cost, profit, and profit margin.
-   Customers whose latest completed order is more than 90 days before
    the latest order date in the dataset.
-   Customer spending groups and customer value segments.

### 5. Window Functions

The script demonstrates:

-   `RANK()` to rank customers by spending.
-   `ROW_NUMBER()` to number orders by date.
-   `DENSE_RANK()` to rank products within product categories.
-   `LAG()` to compare a month's revenue with the previous month.
-   `NTILE()` to divide customers into spending groups.
-   Windowed aggregation to calculate category revenue share and
    cumulative-style revenue output.

### 6. Views and Functions

The project includes two database objects beyond the base tables:

-   **`monthly_sales_summary` view:** intended to present monthly sales,
    revenue, cost, and profit.
-   **`f_order_summary(p_customer_id)` function:** returns
    completed-order revenue for a supplied customer ID. It uses
    `COALESCE` so that a customer with no matching completed orders
    returns zero.

The view is created, queried, and then dropped in the script. The
function is created and called with sample customer IDs.

### 7. Management Reporting

The final reporting section contains queries for:

-   The top five customers by completed-order revenue.
-   The five most frequently occurring products by order count.
-   The product category with the highest calculated profit.
-   The month with the lowest calculated revenue.

## Business Insights Recorded in the SQL Script

The final comments in the SQL file record the following findings and
recommendations. These are presented as the project's documented
observations, not as independently re-run or verified results.

  -----------------------------------------------------------------------
  Area                    Observation recorded in Recommendation recorded
                          the script              in the script
  ----------------------- ----------------------- -----------------------
  Product performance     Mika Blender is noted   Focus more on the Mika
                          as the most ordered     Blender
                          product, with 52        
                          orders; Samsung 55in    
                          Smart TV is noted with  
                          25 orders               

  Category performance    Accessories are         Increase marketing of
                          reported to have the    Accessories and review
                          highest profit margin   costs in Computing and
                          at 41.72%; Mobile       Mobile Phones
                          Phones are noted for    
                          high revenue;           
                          Electronics is reported 
                          to have the lowest      
                          profit margin           

  Customer geography      Nairobi is reported as  Increase sales activity
                          the highest-revenue     in Machakos
                          city, while Machakos is 
                          reported as the lowest, 
                          at KSh 1,803,134        

  Monthly performance     Monthly growth is       Review the factors that
                          described as variable;  contributed to March
                          January 2026 is         2025 performance
                          reported as a 61.82%    
                          loss and March 2025 as  
                          the highest growth at   
                          158.38%                 

  Customer segments       VIP customers are       Increase Retail-segment
                          reported as spending    sales through marketing
                          the most (KSh           and/or discount
                          44,837.96), while       strategy
                          Retail customers spend  
                          the least               

  Customer loyalty        Kamau Mutua is          Use customer-level
                          identified as the       purchasing patterns to
                          customer with the       inform retention
                          highest number of       efforts
                          orders, at 26           
  -----------------------------------------------------------------------


## Conclusion

Duka Retail Sales Analysis demonstrates the use of PostgreSQL to build a
relational retail dataset and perform business-oriented analysis. It
covers database design, data integrity, joins, aggregations, CTEs,
subqueries, window functions, views, and PL/pgSQL functions.

The analysis is intended to help a retail business understand customer
purchasing patterns, product and category performance, sales trends, and
profitability. The documented insights can guide decisions around
product focus, marketing, customer retention, and regional sales
opportunities once the relevant calculations have been validated.

------------------------------------------------------------------------

*Project documentation generated from the supplied `duka.sql` script.
The dataset and analysis described here reflect the contents of that
file.*
