# Internship Week 2 Summary

**Intern:** Deepak Biswas  
**Week:** Week 2  
**Domain:** Data Analytics

---

## Table of Contents

1. [E-Mail 1 - Variance Analysis Task](#e-mail-1---variance-analysis-task)
2. [Work and Response - Task 1 Variance Analysis](#work-and-response---task-1-variance-analysis)
3. [E-Mail 2 - SQL Debugging Task](#e-mail-2---sql-debugging-task)
4. [Work and Response - Task 2 SQL Debugging](#work-and-response---task-2-sql-debugging)
5. [E-Mail 3 - Network and Activity Data Task](#e-mail-3---network-and-activity-data-task)
6. [Work and Response - Task 3 Network and Activity Aggregation](#work-and-response---task-3-network-and-activity-aggregation)
7. [Key Takeaways](#key-takeaways)

---

## E-Mail 1 - Variance Analysis Task

**Subject:** Variance Analysis - Benchmark vs Actual Data

You need to conduct variance analysis to compare benchmark data (`benchmarks.csv`) with current data (`fact_orders.csv`). The goal of this analysis is to identify and quantify the differences or variations between the two datasets. Specifically, you will be comparing the order quantity and delivery quantity from the `fact_orders.csv` file with the benchmark data provided in `benchmarks.csv`.

---

### Datasets Provided

**1. fact_orders.csv**

Contains columns: `order_id`, `order_placement_date`, `mmm_yy`, `customer_id`, `product_name`, `order_qty`, `delivery_qty`

**2. dim_customers.csv**

Contains columns: `customer_id`, `customer_name`, `city`

**3. benchmarks.csv**

Contains columns: `mmm_yy`, `customer_id`, `customer_name`, `city`, `total_order_quantity`, `total_delivery_quantity`

---

### Workflow

1. Review the benchmark data and the current data from `fact_orders.csv`
2. Metrics to compare: Order Quantity and Delivery Quantity
3. Calculate the absolute variance between benchmark data and current data for each metric
4. Calculate the percentage variance between benchmark data and current data for each metric
5. Provide a complete report of the differences between the benchmark data and the main data

---

## Work and Response - Task 1 Variance Analysis

**Tool Used:** Power BI - Power Query, DAX Measures, and Report View

---

### Objective

Perform Variance Analysis between the Benchmark dataset and the Actual operational dataset to analyze and compare two key metrics: Order Quantity and Delivery Quantity.

This comparison evaluates how closely the actual performance aligns with the expected benchmark performance for each customer across different months.

---

### Data Loading and Validation

All three datasets were imported into Power BI Desktop using:

```
Home -> Get Data -> CSV
```

**Validation - fact_orders Table**

| Column               | Data Type    | Status |
|----------------------|--------------|--------|
| order_id             | Text         | Valid  |
| order_placement_date | Date         | Valid  |
| mmm_yy               | Text         | Valid  |
| customer_id          | Whole Number | Valid  |
| product_name         | Text         | Valid  |
| order_qty            | Whole Number | Valid  |
| delivery_qty         | Whole Number | Valid  |

Column profiling confirmed 100% valid values, 0% errors, and 0% empty values across all three tables. No data cleaning was required.

![Data Loading and Validation in Power Query](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_1.png)

---

### Data Modeling - Star Schema

After validating the datasets, relationships were established between the tables.

**Relationship 1**

```
dim_customers[customer_id]  --  1 to Many  -->  fact_orders[customer_id]
```

**Relationship 2**

```
dim_customers[customer_id]  --  1 to Many  -->  benchmarks[customer_id]
```

- Cardinality: One-to-Many
- Cross filter direction: Single

**Model Structure**

```
         dim_customers
              |
    ----------------------
    |                    |
fact_orders          benchmarks
```

| Table         | Role                   |
|---------------|------------------------|
| dim_customers | Dimension table        |
| fact_orders   | Actual data fact table |
| benchmarks    | Benchmark fact table   |

![Star Schema - Data Model View](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_2.png)

---

### DAX Measures Created

All measures were stored in a separate measures table to keep the model organized.

**Actual Quantity Measures**

```dax
-- Measure 1
Recorded Order Qty =
CALCULATE(
    SUM(fact_orders[order_qty]),
    TREATAS(VALUES(benchmarks[mmm_yy]), fact_orders[mmm_yy])
)

-- Measure 2
Recorded Delivery Qty =
CALCULATE(
    SUM(fact_orders[delivery_qty]),
    TREATAS(VALUES(benchmarks[mmm_yy]), fact_orders[mmm_yy])
)
```

The `TREATAS` function ensures that the month filter from the benchmark table is applied to the `fact_orders` table, enabling correct month-wise comparisons.

**Benchmark Quantity Measures**

```dax
-- Measure 3
Benchmark Order Qty = SUM(benchmarks[total_order_quantity])

-- Measure 4
Benchmark Delivery Qty = SUM(benchmarks[total_delivery_quantity])
```

**Order Variance Measures**

```dax
-- Measure 5
Diff Order Qty =
ABS([Benchmark Order Qty] - [Recorded Order Qty])

-- Measure 6
Order % From Benchmark =
DIVIDE([Diff Order Qty], [Benchmark Order Qty])
```

**Delivery Variance Measures**

```dax
-- Measure 7
Diff Delivery Qty =
ABS([Benchmark Delivery Qty] - [Recorded Delivery Qty])

-- Measure 8
Delivery % From Benchmark =
DIVIDE([Diff Delivery Qty], [Benchmark Delivery Qty])
```

![DAX Measures - Measures Table View](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_3.png)

---

### Report Tables Created

**Order Quantity Variance Table**

| Column                 | Type    |
|------------------------|---------|
| mmm_yy                 | Column  |
| customer_id            | Column  |
| customer_name          | Column  |
| city                   | Column  |
| Benchmark Order Qty    | Measure |
| Recorded Order Qty     | Measure |
| Diff Order Qty         | Measure |
| Order % From Benchmark | Measure |

![Order Quantity Variance Report Table](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_4.png)

**Delivery Quantity Variance Table**

| Column                    | Type    |
|---------------------------|---------|
| mmm_yy                    | Column  |
| customer_id               | Column  |
| customer_name             | Column  |
| city                      | Column  |
| Benchmark Delivery Qty    | Measure |
| Recorded Delivery Qty     | Measure |
| Diff Delivery Qty         | Measure |
| Delivery % From Benchmark | Measure |

![Delivery Quantity Variance Report Table](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_5.png)

---

### Analysis Questions and Answers

**Q1. Which customer_id has the largest absolute difference between their recorded order quantity and the benchmark order quantity?**

Formula: `ABS(benchmark_order_qty - recorded_order_qty)`

**Answer: 789903**

---

**Q2. How many customers in the order category have a difference of greater than 3% between their recorded order quantity and the benchmark order quantity?**

Formula: `ABS(benchmark_order_qty - recorded_order_qty) / benchmark_order_qty * 100`

**Answer: 5**

---

**Q3. Which customer_id has the largest absolute difference between their recorded delivery quantity and the benchmark delivery quantity?**

Formula: `ABS(benchmark_delivery_qty - recorded_delivery_qty)`

**Answer: 73011**

---

## E-Mail 2 - SQL Debugging Task

**Subject:** SQL Query Debugging - Urgent

We have some SQL queries created by our previous interns, and it seems they were not debugged. Please download the `gdb080.sql` file and load it into your MySQL Workbench. Open the `sql_queries.docx` file to view the list of queries that need to be debugged. Each query has errors that need to be identified and fixed. Please ensure that you test each query thoroughly after fixing.

---

## Work and Response - Task 2 SQL Debugging

**Tool Used:** MySQL Workbench | Database: gdb080

---

### Question 1 - Unique Customers in Surat

**Question:** How many unique customers are in the city of Surat?

**Original Query (with errors)**

```sql
SELECT
    COUNT(DISTINCT customer_id) AS distinct_customers
FORM gdb080.dim_customers
WHERE city == 'surat';
```

**Errors Identified**

- `FORM` is a typo - correct keyword is `FROM`
- `==` is not valid SQL syntax for equality - correct operator is `=`

**Corrected Query**

```sql
SELECT
    COUNT(DISTINCT customer_id) AS distinct_customers
FROM gdb080.dim_customers
WHERE city = 'surat';
```

**Answer: 11**

---

### Question 2 - Minimum and Maximum Order Quantities per Product

**Question:** What are the minimum and maximum order quantities for each product? What is the highest quantity for the product 'AM Tea 100'?

**Original Query (with errors)**

```sql
SELECT
    p.product_id,
    p.product_name,
    MIN(p.order_qty) as minimum_qty
    MAX(p.order_qty) as maximum_qty
FROM gdb080.fact_order_lines f
JOIN gdb080.dim_products p ON f.product_id = p.product_id
GROUP BY p.product_id;
```

**Errors Identified**

- Missing comma between `minimum_qty` and `MAX(...)` lines
- `MIN` and `MAX` referenced `p.order_qty` from the products table - correct reference is `f.order_qty` from the fact table
- `p.product_name` was missing from the `GROUP BY` clause

**Corrected Query**

```sql
SELECT
    p.product_id,
    p.product_name,
    MIN(f.order_qty) AS minimum_qty,
    MAX(f.order_qty) AS maximum_qty
FROM gdb080.fact_order_lines f
JOIN gdb080.dim_products p
ON f.product_id = p.product_id
GROUP BY
    p.product_id,
    p.product_name;
```

**Answer: 200**

---

### Question 3 - Unfulfilled Orders by Month

**Question:** Generate a report with month name and number of unfulfilled orders in that month. In which month were the unfulfilled orders the highest?

**Original Query (with errors)**

```sql
SELECT
    MONTHNAME(order_placement_date as month_name,
    SUM(order_qty-delivery_qty)
FROM gdb080.fact_orders_lines
GROUP By MONTHNAME(order_placement_date)
ORDER BY unfullfilled_orders DESC;
```

**Errors Identified**

- `as month_name` was placed inside the `MONTHNAME()` function parentheses - the alias must be placed outside the closing parenthesis
- `SUM(order_qty-delivery_qty)` is missing the alias `as unfullfilled_orders`
- Table name `fact_orders_lines` is incorrect - correct name is `fact_order_lines`

**Corrected Query**

```sql
SELECT
    MONTHNAME(order_placement_date) AS month_name,
    SUM(order_qty - delivery_qty) AS unfullfilled_orders
FROM gdb080.fact_order_lines
GROUP BY MONTHNAME(order_placement_date)
ORDER BY unfullfilled_orders DESC;
```

**Answer: May**

---

### Question 4 - Percentage Breakdown of Order Quantity by Category

**Question:** What is the percentage breakdown of order quantity by category? What percentage does the 'food' category account for?

**Original Query (with errors)**

```sql
with total_order_qty_by_category as
(
SELECT
    p.category,
    SUM(f.order_qty) as total_quantity
FROM gdb080.dim_products p
JOIN gdb080.fact_order_lines f ON p.product_id = f.product_id
GROUP BY p.category;
)
SELECT
    category,
    ROUND(100 * total_quantity / SUM(total_quantity) OVER (), 2) AS order_qty_pct
FROM total_order_quantity_by_category
order by order_qty_pct DESC;
```

**Errors Identified**

- Semicolon inside the CTE closing parenthesis terminates the statement early - it must be removed
- The outer `SELECT` referenced `total_order_quantity_by_category` but the CTE was named `total_order_qty_by_category` - the name must match exactly

**Corrected Query**

```sql
WITH total_order_qty_by_category AS
(
    SELECT
        p.category,
        SUM(f.order_qty) AS total_quantity
    FROM gdb080.dim_products p
    JOIN gdb080.fact_order_lines f
    ON p.product_id = f.product_id
    GROUP BY p.category
)
SELECT
    category,
    ROUND(100 * total_quantity / SUM(total_quantity) OVER (), 2) AS order_qty_pct
FROM total_order_qty_by_category
ORDER BY order_qty_pct DESC;
```

**Answer: 12.50%**

---

### Question 5 - Customer On-Time Target Percentage Category

**Question:** Generate a report with customer_id, customer_name, ontime_target_pct, and percentage_category. How many customers fall in the 'Above 90' category?

**Original Query (with errors)**

```sql
SELECT
    customer_id,
    customer_name,
    t.ontime_target_pct,
    CASE
        WHEN t.ontime_target_pct > 90 THEN 'Above 90"
        WHEN t.ontime_target_pct > 80 THEN 'Above 80'
        WHEN t.ontime_target_pct > 70 THEN 'Above 70'
        ELSE "Below 70"
    END AS  percentage_category,
FROM gdb080.dim_targets_orders t
JOIN gdb080.dim_customers c
ON t.customer_id = c.customer_id;
```

**Errors Identified**

- `'Above 90"` uses mismatched quotes - opening single quote with closing double quote
- `"Below 70"` uses double quotes instead of single quotes
- Trailing comma after `percentage_category` before `FROM` causes a syntax error
- `ELSE 'Below 70'` does not match the task specification - correct label is `'Less than 70'`
- `customer_id` and `customer_name` in the `SELECT` are ambiguous - must be prefixed with alias `c`

**Corrected Query**

```sql
SELECT
    c.customer_id,
    c.customer_name,
    t.ontime_target_pct,
    CASE
        WHEN t.ontime_target_pct > 90 THEN 'Above 90'
        WHEN t.ontime_target_pct > 80 THEN 'Above 80'
        WHEN t.ontime_target_pct > 70 THEN 'Above 70'
        ELSE 'Less than 70'
    END AS percentage_category
FROM gdb080.dim_targets_orders t
JOIN gdb080.dim_customers c
ON t.customer_id = c.customer_id;
```

**Updated Query to Count 'Above 90' Rows**

```sql
SELECT
    CASE
        WHEN t.ontime_target_pct > 90 THEN 'Above 90'
        WHEN t.ontime_target_pct > 80 THEN 'Above 80'
        WHEN t.ontime_target_pct > 70 THEN 'Above 70'
        ELSE 'Less than 70'
    END AS percentage_category,
    COUNT(*) AS category_count
FROM gdb080.dim_targets_orders t
JOIN gdb080.dim_customers c
ON t.customer_id = c.customer_id
GROUP BY percentage_category
HAVING percentage_category = 'Above 90';
```

**Answer: 9**

---

### Question 6 - Products by Category with Count

**Question:** List all product categories with product names and total count of products. What is the count of distinct products in the 'Dairy' category?

**Original Query (with errors)**

```sql
SELECT category, GROUP_CONCAT(product_name) AS products
COUNT(*) AS product_count
FROM gdb080.dim_products
GROUP category;
```

**Errors Identified**

- Missing comma between `products` and `COUNT(*)` lines
- `GROUP category` is missing the keyword `BY` - correct syntax is `GROUP BY`

**Corrected Query**

```sql
SELECT
    category,
    GROUP_CONCAT(product_name) AS products,
    COUNT(*) AS product_count
FROM gdb080.dim_products
GROUP BY category;
```

**Answer: 12**

---

### Question 7 - Top 3 Most Demanded Products in Dairy Category

**Question:** What are the top 3 most demanded products in the 'Dairy' category and their order quantities in millions? What is the combined total order quantity for those top 3 products?

**Original Query (with errors)**

```sql
SELECT
    p.product_name,
    ROUND(SUM(f.order_qt) / 1000000, 2) AS order_qty_mln
FROM gdb080.dim_products p
JOIN gdb080.fact_order_lines f
WHERE p.category = 'Dairy'
GROUP BY p.product_name
ORDER BY order_qty_mln DESC
LIMIT 3;
```

**Errors Identified**

- `f.order_qt` is a typo - correct column name is `f.order_qty`
- The `JOIN` clause is missing the `ON` condition

**Corrected Query**

```sql
SELECT
    p.product_name,
    ROUND(SUM(f.order_qty) / 1000000, 2) AS order_qty_mln
FROM gdb080.dim_products p
JOIN gdb080.fact_order_lines f
ON p.product_id = f.product_id
WHERE p.category = 'Dairy'
GROUP BY p.product_name
ORDER BY order_qty_mln DESC
LIMIT 3;
```

**Updated Query to Calculate Combined Total of Top 3**

```sql
SELECT
    ROUND(SUM(order_qty) / 1000000, 2) AS order_qty_mln
FROM (
    SELECT
        p.product_name,
        SUM(f.order_qty) AS order_qty
    FROM gdb080.dim_products p
    JOIN gdb080.fact_order_lines f
    ON p.product_id = f.product_id
    WHERE p.category = 'Dairy'
    GROUP BY p.product_name
    ORDER BY order_qty DESC
    LIMIT 3
) AS top3_products;
```

**Answer: 3.81 Millions**

---

### Question 8 - OTIF Percentage for Vijay Stores

**Question:** Calculate the OTIF percentage for the customer named Vijay Stores.

**Original Query (with errors)**

```sql
SELECT
    c.customer_names,
    ROUND((SUM(f.otif) / COUNT(f.order_id) * 100), 2) AS OTIF_percentage
FROM gdb080.fact_orders_aggregate f
JOIN gdb080.dim_customers c
ON c.customer_id = f.customer_id
GROUP BY c.customer_name
WHERE c.customer_name = "Vijay Stores";
```

**Errors Identified**

- `c.customer_names` is an incorrect column name - correct column is `c.customer_name`
- `WHERE` clause is placed after `GROUP BY` - in SQL, `WHERE` must always appear before `GROUP BY`

**Corrected Query**

```sql
SELECT
    c.customer_name,
    ROUND((SUM(f.otif) / COUNT(f.order_id) * 100), 2) AS OTIF_percentage
FROM gdb080.fact_orders_aggregate f
JOIN gdb080.dim_customers c
ON c.customer_id = f.customer_id
WHERE c.customer_name = "Vijay Stores"
GROUP BY c.customer_name;
```

**Answer: 28.28%**

---

### Question 9 - In Full Percentage by Product

**Question:** What is the percentage of 'in full' for each product and which product has the highest percentage? How many products have an IF percentage greater than 67%?

**Original Query (with errors)**

```sql
WITH product_if_target AS (
SELECT
    p.product_name,
    SUM(CASE WHEN f.in_full = 1 THEN 1 ELSE 0) AS if_count,
    COUNT(f.order_id) AS total_count
FROM gdb080.fact_order_lines f
JOIN gdb080.dim_products p ON p.product_id = f.product_id
GROUP BY p.product_name
)
SELECT
    product_name,
    ROUND(if_count / total_count * 100), 2) AS IF_percentage
FROM product_if_target
order by IF_percentage DESC;
```

**Errors Identified**

- `CASE WHEN f.in_full = 1 THEN 1 ELSE 0` is missing the closing `END` keyword
- `ROUND(if_count / total_count * 100), 2)` has a mismatched parenthesis - the closing parenthesis was placed before `, 2` instead of after it

**Corrected Query**

```sql
WITH product_if_target AS (
    SELECT
        p.product_name,
        SUM(CASE WHEN f.in_full = 1 THEN 1 ELSE 0 END) AS if_count,
        COUNT(f.order_id) AS total_count
    FROM gdb080.fact_order_lines f
    JOIN gdb080.dim_products p
    ON p.product_id = f.product_id
    GROUP BY p.product_name
)
SELECT
    product_name,
    ROUND((if_count / total_count) * 100, 2) AS IF_percentage
FROM product_if_target
ORDER BY IF_percentage DESC;
```

**Updated Query to Count Products with IF Percentage Greater Than 67%**

```sql
WITH product_if_target AS (
    SELECT
        p.product_name,
        SUM(CASE WHEN f.in_full = 1 THEN 1 ELSE 0 END) AS if_count,
        COUNT(f.order_id) AS total_count
    FROM gdb080.fact_order_lines f
    JOIN gdb080.dim_products p
    ON p.product_id = f.product_id
    GROUP BY p.product_name
)
SELECT
    COUNT(*) AS count_of_products
FROM product_if_target
WHERE (if_count / total_count) * 100 > 67;
```

**Answer: 3**

![SQL Debugging - MySQL Workbench Queries](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_2_image_1.png)

![SQL Debugging - Query Results and Validation](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_2_image_2.png)

---

## E-Mail 3 - Network and Activity Data Task

**Subject:** Power Query Transformation - Network and Activity Data

One of our clients in the Network services domain is currently creating a weekly report manually. This process can be automated. I have provided you with two Excel sheets - `network_data.csv` and `activity_data.csv` - containing data related to the client's network operations. Your task is to use Power Query to manipulate and transform the data from these files and create a new table that meets the specified format using pivot, merge, and header manipulation techniques.

---

## Work and Response - Task 3 Network and Activity Aggregation

**Tool Used:** Power BI - Power Query Editor

---

### Objective

Combine two datasets and produce a city-wise summary table showing the count of different network statuses and activity statuses. The final output required a table where each row represents a city, each column represents a specific status, and the values represent the count of records for that status in that city.

---

### Datasets Used

**network_data.csv**

Columns: `city`, `network_id`, `network_status`

Example status values: Connected, Outage, Issue Reported, Pending Activation, Suspended

**activity_data.csv**

Columns: `city`, `network_id`, `activity_status`

Example status values: Maintenance Work, Network Auditing, Network Expansion, New Installation, Shifting

---

### Step 1 - Loading the Data

Both CSV files were loaded into Power BI using:

```
Home -> Get Data -> Text/CSV
```

The Transform Data option was used to open Power Query Editor for all transformations.

![Data Loading - Raw Network and Activity Tables](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_3_image_1.png)

---

### Step 2 - Promoting First Row as Headers

Both tables initially contained generic column names such as Column1, Column2, and Column3. Since the actual column names were present in the first row, the following step was applied to both tables:

```
Home -> Use First Row as Headers
```

---

### Step 3 - Pivoting the Network Data

The `network_data` table contained multiple rows per city representing different network statuses. To convert statuses into columns, the Pivot Column transformation was applied.

Steps performed:

1. Selected the `network_status` column
2. Navigated to: `Transform -> Pivot Column`
3. Values Column set to: `network_id`
4. Aggregation set to: `Count (All)`

**Before Pivot**

| city   | network_id | network_status |
|--------|------------|----------------|
| Mumbai | N1         | Connected      |
| Mumbai | N2         | Outage         |

**After Pivot**

| city   | Connected | Outage | Issue Reported | Pending Activation | Suspended |
|--------|-----------|--------|----------------|--------------------|-----------|
| Mumbai | 3         | 1      | 2              | 0                  | 1         |

The same transformation was then applied to the `activity_data` table using the `activity_status` column to produce a city-wise count of each activity type.

![Pivot Transformation - Network and Activity Data](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_3_image_2.png)

---

### Step 4 - Merging the Two Tables

After pivoting both datasets, they were combined into a single table using the Merge Queries feature.

```
Home -> Merge Queries as New
```

Merge configuration:

- First Table: `activity_data`
- Second Table: `network_data`
- Matching Column: `city`
- Join Type: Left Outer Join

This ensured that all cities from the activity dataset were retained along with their matching network status information.

![Merge Queries - Combining Network and Activity Data](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_3_image_3.png)

---

### Step 5 - Expanding and Finalizing the Output

After merging, a nested `network_data` column appeared in the table. To bring those columns into the main table:

1. Clicked the expand icon on the `network_data` column
2. Selected the following columns: Connected, Issue Reported, Outage, Pending Activation, Suspended
3. Disabled the option to use the original column name as prefix

The final query was renamed to `final_output` and loaded into Power BI using:

```
Home -> Close & Apply
```

**Final Output Table Structure**

| city | Connected | Issue Reported | Outage | Pending Activation | Suspended | Maintenance Work | Network Auditing | Shifting | Network Expansion | New Installation |
|------|-----------|----------------|--------|--------------------|-----------|------------------|------------------|----------|-------------------|------------------|

Each column value represents the count of records belonging to that category within a city.

![Final Output Table - City-wise Network and Activity Summary](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_3_image_4.png)

---

### Analysis Questions and Answers

**Q1. Which city has the lowest frequency of the "Connected" network category?**

**Answer: Jaipur**

---

**Q2. How many cities have reported the network "Outage" category more than 5 times?**

**Answer: 3**

---

**Q3. What is the median frequency of the "Network Expansion" category across all cities?**

**Answer: 5**

---

**Q4. What is the total frequency of the "Shifting" category in Ahmedabad city?**

**Answer: 11**

---

**Q5. What is the average frequency of the "Suspended" category across all cities?**

**Answer: 3.7**

---

### Additional Reference

![Week 2 - Overall Analysis and Dashboard View](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_4_image_1.png)

![Week 2 - Supporting Output Reference](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_4_image_2.png)

---

## Key Takeaways

- Variance analysis using `TREATAS` in DAX enables accurate cross-table month-wise filtering even when tables are not directly related on the same column.
- A star schema connecting a single dimension table to two fact tables is an effective pattern for benchmark versus actual comparisons.
- SQL debugging requires careful attention to syntax rules: keyword ordering such as `WHERE` before `GROUP BY`, correct use of aliases, matching quote styles, and closing all open parentheses and `CASE` statements with the `END` keyword.
- Power Query pivot and merge operations can fully automate the production of structured city-level summary reports that would otherwise require manual effort each week.
- Separating DAX measures into a dedicated measures table keeps the Power BI model organized and easier to maintain as the number of calculations grows.

---

*Deepak Biswas - Week 2 Internship Report*
