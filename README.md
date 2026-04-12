# Data Analytics Internship — AtliQ Technologies

**Intern:** Deepak Biswas
**Domain:** Data Analytics
**Mode:** Remote
**Duration:** 4 Weeks

---

## Table of Contents

1. [Internship Overview](#internship-overview)
2. [Tools and Technologies](#tools-and-technologies)
3. [Week 1 — Data Cleaning, Normalization, and Scrum Training](#week-1--data-cleaning-normalization-and-scrum-training)
4. [Week 2 — Variance Analysis, SQL Debugging, Report Automation, and 5G Impact Analysis](#week-2--variance-analysis-sql-debugging-report-automation-and-5g-impact-analysis)
5. [Week 3 — Requirements Analysis and Dashboard Mockup Design](#week-3--requirements-analysis-and-dashboard-mockup-design)
6. [Week 4 — Shield Insurance Power BI Dashboard](#week-4--shield-insurance-power-bi-dashboard)
7. [Key Learnings](#key-learnings)
8. [Conclusion](#conclusion)

---

## Internship Overview

As a Data Analyst Intern at AtliQ Technologies, I worked alongside experienced data professionals on real client projects spanning data cleaning, SQL debugging, variance analysis, Power Query automation, dashboard design, and full-scale Power BI development.

Each week introduced progressively more complex tasks — beginning with foundational data quality work and advancing through analytical modeling, stakeholder communication, and client-ready dashboard delivery. All work was conducted remotely, with tasks assigned through structured email briefs from the Head of Data Analytics.

The internship concluded with the delivery of a complete Power BI dashboard for Shield Insurance as a Proof of Concept pilot project.

---

## Tools and Technologies

| Tool | Purpose |
|------|---------|
| Power BI | Data modeling, DAX measures, dashboard development |
| Power Query | Data cleaning, transformation, pivoting, and merging |
| DAX (Data Analysis Expressions) | Calculated columns, measures, and analytical logic |
| DAX Studio | DAX query optimization and testing |
| MySQL Workbench | SQL query debugging and validation |
| Microsoft Excel | Data normalization, feature list documentation |
| PowerPoint | Insights presentation for client stakeholders |
| Figma | Dashboard mockup and wireframe design |

---

## Week 1 — Data Cleaning, Normalization, and Scrum Training

Week 1 established the foundation of the internship. The tasks covered three distinct areas: cleaning and analyzing an HR attendance dataset in Power BI, transforming a denormalized order dataset into a star schema, and completing mandatory Scrum methodology training.

---

### Task 1 — HR Attendance Data: Cleaning and Analysis

**Tool Used:** Power BI — Power Query and DAX

#### Data Cleaning Steps

The raw `attendance_data.csv` file required the following transformations before analysis could begin:

- Removed all duplicate rows from the dataset
- Standardized all date values to `YYYY-MM-DD` format and extracted `Month Name` and `Day Type` as new columns
- Stripped trailing `@` characters and other special characters from Employee ID values
- Applied Title Case formatting to all employee name entries
- Replaced full-form status values with standardized abbreviations using a Power Query mapping transformation

| Full Form | Abbreviation |
|-----------|--------------|
| Work From Office | WFO |
| Work From Home | WFH |
| Birthday Leave | BL |
| Menstrual Leave | ML |
| Paid Leave | PL |
| Sick Leave | SL |
| Weekly Off | WO |

![Data Cleaning - Power Query View](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_1_T_1_image_1.png)

![Data Model - Power BI View](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_1_T_1_image_2.png)

---

#### Ad Hoc Analysis — DAX Measures and Results

**Question 1: Total count of distinct employees**

```dax
Total Rows = COUNTROWS(attendance_data)
Count Distinct Employee Names = DISTINCTCOUNT(attendance_data[name])
Distinct Employee IDs = DISTINCTCOUNT('attendance_data'[employee_id])
```

**Result:** 99 distinct employees by name | 74 distinct employee IDs

---

**Question 2: WFH percentage in May**

```dax
WFH Records May =
CALCULATE(COUNTROWS('attendance_data'),
    MONTH('attendance_data'[date]) = 5,
    'attendance_data'[status_abbreviations] = "WFH")

WFO Records May =
CALCULATE(COUNTROWS('attendance_data'),
    MONTH('attendance_data'[date]) = 5,
    'attendance_data'[status_abbreviations] = "WFO")

Total Present Days May = [WFH Records May] + [WFO Records May]

WFH % May = DIVIDE([WFH Records May], [Total Present Days May])
```

**Result:** WFH % for May = **11.49%**

---

**Question 3: Highest attendance day in June**

```dax
Present Records June =
CALCULATE(COUNTROWS('attendance_data'),
    'attendance_data'[Month Name] = "June",
    'attendance_data'[status_abbreviations] IN { "WFO", "WFH" })

Total Records June =
CALCULATE(COUNTROWS('attendance_data'),
    'attendance_data'[Month Name] = "June")

Attendance % June = DIVIDE([Present Records June], [Total Records June])
```

| Day | Attendance Percentage |
|-----|-----------------------|
| Tuesday | 97.53% — Highest |
| Wednesday | 95.43% |
| Monday | 93.41% |
| Sunday | 14.06% — Lowest |

---

**Question 4: Employees with WFH greater than 10% in April**

```dax
WFH Records April =
CALCULATE(COUNTROWS('attendance_data'),
    'attendance_data'[status_abbreviations] = "WFH",
    FILTER('attendance_data', MONTH('attendance_data'[date]) = 4))

WFO Records April =
CALCULATE(COUNTROWS('attendance_data'),
    MONTH('attendance_data'[date]) = 4,
    'attendance_data'[status_abbreviations] = "WFO")

WFH % April = DIVIDE([WFH Records April], [WFH Records April] + [WFO Records April])

Employees WFH Greater Than 10% April =
COUNTROWS(FILTER(VALUES('attendance_data'[name]), [WFH % April] > 0.10))
```

**Result:** 16 employees had a WFH percentage greater than 10% in April

---

#### Dashboard Summary

| Metric | Value |
|--------|-------|
| Total Records | 6,208 |
| Distinct Employees by Name | 99 |
| Distinct Employee IDs | 74 |
| WFH Percentage — May | 11.49% |
| Employees with WFH Greater Than 10% — April | 16 |
| Highest Attendance Day — June | Tuesday at 97.53% |

![Employee Attendance Analysis Dashboard](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_1_T_1_image_3.png)

---

### Task 2 — Data Normalization: Star Schema Design

**Tool Used:** Power BI — Power Query and Model View

#### Objective

Transform the denormalized `fact_order_lines.csv` file into a normalized star schema by separating it into appropriate fact and dimension tables.

#### Source Data Structure

The original dataset was a single table containing all of the following in one place:

| Category | Columns |
|----------|---------|
| Order Information | order_id, order_placement_date, mmm_yy, week_no |
| Customer Information | customer_id, customer_name, city |
| Product Information | product_id, product_name, category |
| Measures | order_qty, delivery_qty |

![Denormalized Source Table](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_1_T_2_image_1.png)

#### Dimension Tables Created

**dim_customers** — `customer_id`, `customer_name`, `city` (duplicates removed)

**dim_products** — `product_id`, `product_name`, `category` (duplicates removed)

**dim_dates** — `date`, `mmm_yy`, `week_no` (duplicates removed)

![Normalized Output — Fact and Dimension Tables](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_1_T_2_image_2.png)

#### Fact Table

**fact_orders** — `order_id`, `order_placement_date`, `customer_id`, `product_id`, `order_qty`, `delivery_qty`
Each row represents a unique transaction record with no duplicates removed.

![Power Query — Fact and Dimension Table Setup](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_1_T_2_image_3.png)

#### Star Schema Relationships

```
dim_customers[customer_id]  --  1 to Many  -->  fact_orders[customer_id]
dim_products[product_id]    --  1 to Many  -->  fact_orders[product_id]
dim_dates[date]             --  1 to Many  -->  fact_orders[order_placement_date]
```

- Cardinality: One-to-Many
- Cross filter direction: Single

![Star Schema — Model View in Power BI](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_1_T_2_image_4.png)

---

### Task 3 — Scrum Methodology Training

**Platform:** [scrumtrainingseries.com](https://scrumtrainingseries.com/)
**Assessment Result:** Passed with 90%

The mandatory Scrum training covered five core modules:

![Scrum Training Series — Website Overview](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_1_T_3_image_1.png)

**Module 1 — Scrum in Small vs Large Organizations**

Scrum works most effectively when a single Product Owner holds full decision-making authority, teams are cross-functional, and a single unified backlog captures all priorities. Scaling challenges arise when teams are fragmented, velocity is prioritized over customer value, and developers lose direct contact with real users.

**Module 2 — Backlog Refinement**

Product Backlog Items are clarified and decomposed before Sprint Planning using the INVEST standard: Independent, Negotiable, Valuable, Estimable, Small, and Testable. No commitments are made during refinement — commitment happens at Sprint Planning.

**Module 3 — Sprint Planning**

Sprint Planning covers two topics: what the team will deliver (Sprint Goal and selected PBIs) and how they will deliver it (Sprint Backlog with tasks and design steps). Tasks are not pre-assigned — team members self-select work during execution.

**Module 4 — Daily Scrum**

A 15-minute daily event owned by the Development Team, not the Scrum Master. Supports continuous integration, test-driven development, and pair programming practices. Side discussions are captured as sidebar topics and addressed separately.

**Module 5 — Sprint Review**

Completed work is demonstrated to stakeholders at the end of each sprint. Only fully done PBIs are accepted — partially completed items are not counted. All stakeholder feedback is added to the Product Backlog and prioritized by the Product Owner.

---

## Week 2 — Variance Analysis, SQL Debugging, Report Automation, and 5G Impact Analysis

Week 2 introduced client-facing analytical work. Tasks scaled significantly in complexity, requiring cross-table DAX modeling, hands-on SQL debugging, Power Query automation for a live client report, and a structured insights presentation for a telecom client's 5G rollout.

---

### Task 1 — Variance Analysis

**Tool Used:** Power BI — Power Query, DAX, and Report View

#### Objective

Compare benchmark performance data against actual operational data to quantify the variance in Order Quantity and Delivery Quantity for each customer across different months.

#### Data Model

Three datasets were imported and connected in a star schema:

```
         dim_customers
              |
    ----------------------
    |                    |
fact_orders          benchmarks
```

| Table | Role |
|-------|------|
| dim_customers | Shared dimension table |
| fact_orders | Actual performance fact table |
| benchmarks | Target benchmark fact table |

![Data Loading and Validation in Power Query](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_1.png)

![Star Schema — Data Model View](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_2.png)

#### DAX Measures

```dax
-- Actual Quantities (TREATAS ensures month filter from benchmarks applies to fact_orders)
Recorded Order Qty =
CALCULATE(SUM(fact_orders[order_qty]),
    TREATAS(VALUES(benchmarks[mmm_yy]), fact_orders[mmm_yy]))

Recorded Delivery Qty =
CALCULATE(SUM(fact_orders[delivery_qty]),
    TREATAS(VALUES(benchmarks[mmm_yy]), fact_orders[mmm_yy]))

-- Benchmark Quantities
Benchmark Order Qty = SUM(benchmarks[total_order_quantity])
Benchmark Delivery Qty = SUM(benchmarks[total_delivery_quantity])

-- Variance — Absolute
Diff Order Qty = ABS([Benchmark Order Qty] - [Recorded Order Qty])
Diff Delivery Qty = ABS([Benchmark Delivery Qty] - [Recorded Delivery Qty])

-- Variance — Percentage
Order % From Benchmark = DIVIDE([Diff Order Qty], [Benchmark Order Qty])
Delivery % From Benchmark = DIVIDE([Diff Delivery Qty], [Benchmark Delivery Qty])
```

![DAX Measures — Measures Table View](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_3.png)

![Order Quantity Variance Report Table](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_4.png)

![Delivery Quantity Variance Report Table](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_1_image_5.png)

#### Key Results

| Question | Answer |
|----------|--------|
| Customer with largest absolute order quantity difference | 789903 |
| Customers with order variance greater than 3% | 5 |
| Customer with largest absolute delivery quantity difference | 73011 |

---

### Task 2 — SQL Query Debugging

**Tool Used:** MySQL Workbench | Database: gdb080

#### Objective

Identify and fix syntax and logical errors in nine SQL queries written by previous interns, then validate each corrected query against the database.

---

**Query 1 — Unique Customers in Surat**

Errors: `FORM` typo, `==` operator invalid in SQL

```sql
-- Corrected
SELECT COUNT(DISTINCT customer_id) AS distinct_customers
FROM gdb080.dim_customers
WHERE city = 'surat';
```

**Answer: 11**

---

**Query 2 — Min and Max Order Quantities per Product**

Errors: Missing comma between SELECT lines, wrong table reference for order_qty, missing column in GROUP BY

```sql
-- Corrected
SELECT p.product_id, p.product_name,
    MIN(f.order_qty) AS minimum_qty,
    MAX(f.order_qty) AS maximum_qty
FROM gdb080.fact_order_lines f
JOIN gdb080.dim_products p ON f.product_id = p.product_id
GROUP BY p.product_id, p.product_name;
```

**Answer: Highest quantity for AM Tea 100 = 200**

---

**Query 3 — Unfulfilled Orders by Month**

Errors: Alias inside function parentheses, missing alias on SUM, wrong table name

```sql
-- Corrected
SELECT
    MONTHNAME(order_placement_date) AS month_name,
    SUM(order_qty - delivery_qty) AS unfullfilled_orders
FROM gdb080.fact_order_lines
GROUP BY MONTHNAME(order_placement_date)
ORDER BY unfullfilled_orders DESC;
```

**Answer: May**

---

**Query 4 — Order Quantity Percentage by Category**

Errors: Semicolon inside CTE parentheses terminates the statement early, CTE name mismatch in outer SELECT

```sql
-- Corrected
WITH total_order_qty_by_category AS (
    SELECT p.category, SUM(f.order_qty) AS total_quantity
    FROM gdb080.dim_products p
    JOIN gdb080.fact_order_lines f ON p.product_id = f.product_id
    GROUP BY p.category
)
SELECT category,
    ROUND(100 * total_quantity / SUM(total_quantity) OVER (), 2) AS order_qty_pct
FROM total_order_qty_by_category
ORDER BY order_qty_pct DESC;
```

**Answer: Food category = 12.50%**

---

**Query 5 — Customer On-Time Target Percentage Category**

Errors: Mismatched quotes on THEN value, double quotes on ELSE value, trailing comma before FROM, ambiguous column references, incorrect ELSE label

```sql
-- Corrected
SELECT c.customer_id, c.customer_name, t.ontime_target_pct,
    CASE
        WHEN t.ontime_target_pct > 90 THEN 'Above 90'
        WHEN t.ontime_target_pct > 80 THEN 'Above 80'
        WHEN t.ontime_target_pct > 70 THEN 'Above 70'
        ELSE 'Less than 70'
    END AS percentage_category
FROM gdb080.dim_targets_orders t
JOIN gdb080.dim_customers c ON t.customer_id = c.customer_id;
```

**Answer: 9 customers in the Above 90 category**

---

**Query 6 — Products by Category with Count**

Errors: Missing comma between SELECT lines, `GROUP category` missing the BY keyword

```sql
-- Corrected
SELECT category,
    GROUP_CONCAT(product_name) AS products,
    COUNT(*) AS product_count
FROM gdb080.dim_products
GROUP BY category;
```

**Answer: 12 distinct products in the Dairy category**

---

**Query 7 — Top 3 Most Demanded Products in Dairy**

Errors: `f.order_qt` typo, JOIN missing ON condition

```sql
-- Corrected
SELECT p.product_name,
    ROUND(SUM(f.order_qty) / 1000000, 2) AS order_qty_mln
FROM gdb080.dim_products p
JOIN gdb080.fact_order_lines f ON p.product_id = f.product_id
WHERE p.category = 'Dairy'
GROUP BY p.product_name
ORDER BY order_qty_mln DESC
LIMIT 3;
```

**Answer: Combined total of top 3 = 3.81 Million**

---

**Query 8 — OTIF Percentage for Vijay Stores**

Errors: Wrong column name `customer_names`, WHERE placed after GROUP BY

```sql
-- Corrected
SELECT c.customer_name,
    ROUND((SUM(f.otif) / COUNT(f.order_id) * 100), 2) AS OTIF_percentage
FROM gdb080.fact_orders_aggregate f
JOIN gdb080.dim_customers c ON c.customer_id = f.customer_id
WHERE c.customer_name = "Vijay Stores"
GROUP BY c.customer_name;
```

**Answer: 28.28%**

---

**Query 9 — In Full Percentage by Product**

Errors: CASE statement missing END keyword, misplaced parenthesis in ROUND function

```sql
-- Corrected
WITH product_if_target AS (
    SELECT p.product_name,
        SUM(CASE WHEN f.in_full = 1 THEN 1 ELSE 0 END) AS if_count,
        COUNT(f.order_id) AS total_count
    FROM gdb080.fact_order_lines f
    JOIN gdb080.dim_products p ON p.product_id = f.product_id
    GROUP BY p.product_name
)
SELECT product_name,
    ROUND((if_count / total_count) * 100, 2) AS IF_percentage
FROM product_if_target
ORDER BY IF_percentage DESC;
```

**Answer: 3 products with IF percentage greater than 67%**

![SQL Debugging — MySQL Workbench Queries](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_2_image_1.png)

![SQL Debugging — Query Results and Validation](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_2_image_2.png)

---

### Task 3 — Network and Activity Report Automation

**Tool Used:** Power BI — Power Query Editor

#### Objective

Automate the production of a city-level summary table that a client in the Network Services domain was generating manually each week. The output required a pivot table combining network status counts and activity status counts per city.

#### Transformation Steps

**Step 1 — Load and promote headers**

Both `network_data.csv` and `activity_data.csv` were loaded into Power Query. The first row of each table was promoted to column headers using `Home -> Use First Row as Headers`.

**Step 2 — Pivot the network data**

The `network_status` column was pivoted with `network_id` as the values column and Count (All) as the aggregation, converting status values into columns.

**Step 3 — Pivot the activity data**

The same pivot transformation was applied to `activity_data` using the `activity_status` column.

![Data Loading — Raw Network and Activity Tables](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_3_image_1.png)

![Pivot Transformation — Network and Activity Data](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_3_image_2.png)

**Step 4 — Merge the two pivoted tables**

```
Home -> Merge Queries as New
First Table: activity_data | Second Table: network_data
Matching Column: city | Join Type: Left Outer
```

![Merge Queries — Combining Network and Activity Data](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_3_image_3.png)

**Step 5 — Expand and finalize**

The nested `network_data` column was expanded to bring all network status columns into the main table. The final query was renamed `final_output` and loaded into Power BI.

![Final Output Table — City-wise Network and Activity Summary](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_3_image_4.png)

#### Key Results

| Question | Answer |
|----------|--------|
| City with lowest Connected frequency | Jaipur |
| Cities with Outage count greater than 5 | 3 |
| Median frequency of Network Expansion across cities | 5 |
| Total Shifting frequency in Ahmedabad | 11 |
| Average Suspended frequency across all cities | 3.7 |

---

### Task 4 — Wavecon 5G Impact Analysis

**Tool Used:** Power BI Dashboard + PowerPoint Presentation

#### Executive Summary

| KPI | Before 5G | After 5G | Change |
|-----|-----------|----------|--------|
| Total Revenue | Rs. 15.977B | Rs. 15.897B | -0.50% |
| ARPU | Rs. 190.2 | Rs. 211.3 | +11.05% |
| Total Active Users | 84.4M | 77.4M | -8.28% |
| Unsubscribed Users | 5.6M | 7.0M | +23.50% |

Revenue per customer increased, but the total user base shrank. If churn continues, long-term revenue and market share are at significant risk.

#### Key Insights

- Revenue declined marginally (-0.50%) despite a strong ARPU increase (+11.05%), indicating that volume loss is offsetting per-user revenue gains
- Total Active Users fell by 8.28% — the most critical operational indicator post-launch
- Cancellations surged 23.5%, concentrated in specific cities, pointing to localised network or competitive causes
- Delhi (-2.83%) and Chennai (-2.59%) recorded the steepest revenue declines; Lucknow (+1.82%), Gurgaon (+1.51%), and Patna (+1.48%) showed growth
- Revenue is heavily concentrated in metros — Mumbai (~Rs. 4.9B), Delhi (~Rs. 3.9B), and Kolkata (~Rs. 3.8B) — making any metro decline disproportionately impactful
- Wavecon's market share remained at approximately 19-21% against market leader PIO at ~35%, indicating 5G alone did not shift competitive positioning

#### Recommendations

- Prioritize network reliability improvements in high-churn cities, specifically Delhi, Chennai, and Ahmedabad
- Launch targeted retention campaigns with discounted 5G migration, bill credits, and expedited SIM swap programs
- Redesign tariff bundles to include clear 5G upgrade paths and value-added services for different customer segments
- Apply defensive pricing and service improvements in metros while driving adoption with promotional offers in Tier-2 and Tier-3 cities where conditions are more favorable
- Implement early-warning dashboards tracking TAU and unsubscribed users by cohort and city to enable faster intervention

![Wavecon 5G Impact — Dashboard and KPI Overview](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_4_image_1.png)

![Wavecon 5G Impact — Supporting Analysis and Slides](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_2_T_4_image_2.png)

---

## Week 3 — Requirements Analysis and Dashboard Mockup Design

Week 3 marked the beginning of the Shield Insurance pilot project. The focus shifted from analytical execution to structured planning — converting a client email into a formal feature list, understanding the data model, and designing a multi-page dashboard mockup before any development began.

---

### Task 1 — Feature List and Data Understanding

**Tool Used:** Microsoft Excel

#### Objective

Convert unstructured client requirements from a business email into a structured feature list that would serve as the project benchmark throughout development.

#### Data Understanding

The dataset consisted of two dimension tables and two fact tables:

| Table | Key Columns | Purpose |
|-------|-------------|---------|
| dim_customer | customer_code, dob, city | Customer segmentation by age and location |
| dim_date | date, mmm_yy, day_type, week_no | Time-based analysis |
| dim_policies | policy_id, base_cover, base_premium_amt | Policy-level insights |
| fact_premiums | date, customer_code, policy_id, sales_mode, final_premium_amt | Revenue and sales analysis |
| fact_settlements | age, settlement% | Risk and settlement analysis by age |

#### Feature List

| Sno | Feature | Comments | Priority |
|-----|---------|----------|----------|
| 1 | Total customers, total revenue, daily revenue growth, daily customer growth as key metrics | To monitor all crucial business metrics | High |
| 2 | Month-over-month change % on key metrics | Tracking MoM change in policies provides valuable performance trend insights | High |
| 3 | Segment customers by age groups: 18-24, 25-30, 31-40, 41-50, 51-65, 65+ | To categorize customers and better understand behavior and preferences | High |
| 4 | Total revenue split by age group and city | To identify the most profitable demographics and geographical areas | High |
| 5 | Total customers split by age group and city | To understand customer distribution for targeted product and marketing strategies | High |
| 6 | Daily customer growth trend by month | To monitor growth trends and adjust marketing efforts accordingly | Low |
| 7 | Revenue and daily revenue growth trend by month | To track financial performance and identify fluctuations over time | Low |
| 8 | Toggle switch between revenue trend graph and customer trend graph | Enhances user experience by providing a more efficient view of trend data | Low |
| 9 | Filters for sales mode, age group, city, month, and policy ID | Enables targeted analysis through flexible filtering | High |
| 10 | Separate page for sales mode analysis | A dedicated page for in-depth sales mode analysis | High |
| 11 | Total customer split percentage by sales mode | Insight into which modes are most effective in customer acquisition | High |
| 12 | Total revenue split percentage by sales mode | Insight into which modes are most effective in revenue generation | High |
| 13 | Trend of sales mode over month | To track how sales mode preferences shift over time | Low |
| 14 | Separate page for age group analysis | A dedicated page for in-depth age group analysis | High |
| 15 | Age group vs. expected settlement | To understand expected annual settlement amounts by age group | High |
| 16 | Age group vs. sales mode | To understand preferred sales modes across different age groups | Low |
| 17 | Age group vs. policy preference | To understand which policies are preferred by different age groups | High |

![Feature List — Benchmark Document](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_3_T_1_image_1.png)

---

### Task 2 — Dashboard Mockup Design

**Tool Used:** Figma

#### Objective

Design a visual wireframe of the final dashboard across multiple pages before any Power BI development began. The mockup served as a client-reviewable blueprint that aligned stakeholder expectations with the planned technical execution.

#### Page Structure

**Page 1 — Overview**

Core KPIs displayed prominently: total customers, total revenue, daily revenue growth, and daily customer growth. Month-over-month change percentages shown alongside each metric. Toggle switch for switching between revenue and customer trend graphs. Global filter panel for sales mode, age group, city, month, and policy ID.

**Page 2 — Sales Mode Analysis**

Customer count and revenue split as percentages by sales mode (Offline-Agent, Offline-Direct, Online-App, Online-Website). Monthly trend line for each sales mode.

**Page 3 — Age Group Analysis**

Expected settlement percentage by age group. Sales mode preference breakdown by age group. Policy preference distribution by age group.

![Dashboard Mockup — Overview Page Layout](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_3_T_2_image_2.png)

![Dashboard Mockup — Sales Mode Analysis Page](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_3_T_2_image_3.png)

![Dashboard Mockup — Age Group Analysis Page](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_3_T_2_image_4.png)

---

## Week 4 — Shield Insurance Power BI Dashboard

Week 4 was the culmination of the internship. All prior skills — data modeling, DAX development, stakeholder documentation, and dashboard design — were applied to deliver the final Power BI dashboard for Shield Insurance as a complete Proof of Concept pilot project.

---

### Project Overview

**Client:** Shield Insurance Company
**Deliverable:** Power BI Business Intelligence Dashboard (Pilot Project)
**Scope:** 17 business requirements across 3 dashboard views

---

### Data Preparation

**Cleaning steps performed in Excel and Power Query:**

- Removed empty columns from `fact_settlements`
- Split the `sales_mode` column in `fact_premiums` by delimiter `'-'` and renamed into two columns: Mode (Online/Offline) and Mode (Through Medium)
- Verified and corrected data types across all tables, including the `Settlement %` column

---

### Data Model — Star Schema

Built a clean star schema with the following calculated columns:

**dim_customer:**

```dax
Age = 2023 - YEAR(dim_customer[dob].[Date])

AgeGroup = SWITCH(TRUE(),
    dim_customer[Age] >= 18 && dim_customer[Age] <= 24, "18-24",
    dim_customer[Age] >= 25 && dim_customer[Age] <= 30, "25-30",
    dim_customer[Age] >= 31 && dim_customer[Age] <= 40, "31-40",
    dim_customer[Age] >= 41 && dim_customer[Age] <= 50, "41-50",
    dim_customer[Age] >= 51 && dim_customer[Age] <= 65, "51-65",
    "65+")

SettlementINDecimal = RELATED(fact_settlements[settlement %])
```

**dim_date:**

```dax
Month = FORMAT(dim_date[date].[Date], "MMM")

SortedBy = SWITCH(TRUE(),
    dim_date[Month]="Nov", 1,
    dim_date[Month]="Dec", 2,
    dim_date[Month]="Jan", 3,
    dim_date[Month]="Feb", 4,
    dim_date[Month]="Mar", 5, 6)
```

![Data Model — Star Schema in Power BI](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_1.png)

---

### DAX Measures

```dax
-- Core Metrics
Total Customers    = COUNT(dim_customer[customer_code])
Total Revenue      = SUM(fact_premiums[final_premium_amt(INR)])

-- Daily Growth
DRG = VAR _tot_rev = [Total Revenue]
      VAR _date    = DISTINCTCOUNT(dim_date[date])
      RETURN DIVIDE(_tot_rev, _date, 0)

DCG = VAR _tot_cus = [Total Customers]
      VAR _date    = DISTINCTCOUNT(dim_date[date])
      RETURN DIVIDE(_tot_cus, _date, 0)

-- Previous Month Baselines
LM Customers = CALCULATE([Total Customers], PREVIOUSMONTH(dim_date[date]))
LM Revenue   = CALCULATE([Total Revenue],   PREVIOUSMONTH(dim_date[date]))
LM DCG       = CALCULATE([DCG],             PREVIOUSMONTH(dim_date[date]))
LM DRG       = CALCULATE([DRG],             PREVIOUSMONTH(dim_date[date]))

-- Month-over-Month Change %
%CustomerChange = DIVIDE([Total Customers] - [LM Customers], [LM Customers], 0)
%RevenueChange  = DIVIDE([Total Revenue]   - [LM Revenue],   [LM Revenue],   0)
%DCGChange      = DIVIDE([DCG]             - [LM DCG],       [LM DCG],       0)
%DRGChange      = DIVIDE([DRG]             - [LM DRG],       [LM DRG],       0)

-- Split Percentages
Cus% = VAR _tot = CALCULATE([Total Customers], REMOVEFILTERS(fact_premiums))
       RETURN DIVIDE([Total Customers], _tot)

Rev% = VAR _tot = CALCULATE([Total Revenue], REMOVEFILTERS(fact_premiums))
       RETURN DIVIDE([Total Revenue], _tot)

-- Expected Settlement
Excepted Settlement = ROUND(
    SUMX(fact_premiums,
        fact_premiums[final_premium_amt(INR)] * (1 + RELATED(dim_customer[SettlementINDecimal]))
    ), 0)
```

---

### Dashboard Views

#### Overview Page

![Overview Dashboard](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_2.png)

#### Sales Mode Analysis Page

![Sales Mode Analysis](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_3.png)

#### Age Group Analysis Page

![Age Group Analysis](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_4.png)

#### Documentation and DAX Metrics List

![DAX Metrics and Documentation](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_5.png)

---

### Key Insights and Findings

| # | Insight |
|---|---------|
| 1 | Shield Insurance serves **26,841 customers** across all cities |
| 2 | Total premium revenue generated: **Rs. 989.25 Million** |
| 3 | Delhi leads all cities with **11,007 customers** and **Rs. 401.57 Million** in revenue |
| 4 | The **31-40 age group** is the largest segment — **11,171 customers** contributing **Rs. 343.76 Million** |
| 5 | March 2023 recorded exceptional growth — **+85% revenue** and **+82% customer** increase month-over-month |
| 6 | April 2023 saw a sharp decline — **-41.73% revenue** and **-41.41% customers** |
| 7 | The Offline Agent channel dominates with **55.41% of customers** and **55.67% of revenue** |
| 8 | Revenue across sales modes is balanced, ranging from **12.60% to 16.27%** |
| 9 | Most popular policy by customers: `POL4321HEL` — chosen by **4,434 customers** |
| 10 | Highest revenue policy: `POL2005HEL` — generated **Rs. 324.26 Million** |

### Recommendations

- **Invest in digital channels** — Scaling Online-App and Online-Website acquisition can diversify revenue and reduce dependency on offline agents
- **Focus on the 31-40 age group** — The highest-value segment deserves tailored policy offerings and retention-focused marketing
- **Investigate the April 2023 decline** — Root cause analysis is needed to prevent recurring monthly drops
- **Promote high-performing policies** — Amplify marketing for `POL4321HEL` (highest adoption) and `POL2005HEL` (highest revenue)

---

## Key Learnings

- Data cleaning is foundational. No analysis is reliable without first resolving data quality issues at the source
- DAX functions like `TREATAS`, `PREVIOUSMONTH`, `REMOVEFILTERS`, and `SUMX` enable powerful cross-table analytical logic that is both reusable and maintainable
- Star schema normalization reduces redundancy, improves query performance, and scales more effectively than denormalized single-table designs
- SQL debugging requires systematic attention to keyword ordering, alias consistency, quote matching, and correct parenthesis closure — small syntax errors produce large analytical failures
- Power Query pivot and merge operations can fully automate repetitive manual reporting tasks, saving hours of weekly effort
- A structured feature list created before development begins prevents scope creep and ensures all client requirements are met
- Dashboard mockups aligned with stakeholder sign-off before development significantly reduce rework during later stages
- Business KPIs must always be analyzed together — a positive unit metric like ARPU can mask a deteriorating volume position if evaluated in isolation
- City and segment-level breakdowns are essential in multi-market analyses, as national averages frequently conceal regional variation that drives strategic decisions

---

## Conclusion

This internship represented a complete end-to-end data analytics engagement — from raw data cleaning in Week 1 through SQL debugging, report automation, and stakeholder presentations in Week 2, structured project planning in Week 3, and final dashboard delivery in Week 4.

The culminating deliverable — the Shield Insurance Power BI Pilot Dashboard — addressed all 17 client requirements across three analytical views and was accompanied by a full DAX metrics list and user documentation. The project demonstrated the ability to translate business requirements into a technical solution, communicate findings clearly to non-technical stakeholders, and deliver production-ready analytical work under real project constraints.

---

<div align="center">

**Deepak Biswas** | Data Analyst Intern
**AtliQ Technologies** | Remote Internship

</div>
