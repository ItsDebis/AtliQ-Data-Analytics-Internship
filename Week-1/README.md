# Internship Week 1 Summary

**Intern:** Deepak Biswas  
**Week:** Week 1  
**Domain:** Data Analytics

---

## Table of Contents

1. [E-Mail 1 - HR Attendance Task](#e-mail-1---hr-attendance-task)
2. [Work and Response - Task 1 Data Cleaning](#work-and-response---task-1-data-cleaning)
3. [Work and Response - Task 2 Analysis](#work-and-response---task-2-analysis)
4. [Dashboard Summary](#dashboard-summary)
5. [E-Mail 2 - Data Normalization Task](#e-mail-2---data-normalization-task)
6. [Work and Response - Data Normalization](#work-and-response---data-normalization)
7. [E-Mail 3 - SCRUM Training](#e-mail-3---scrum-training)
8. [Work and Response - SCRUM Learnings](#work-and-response---scrum-learnings)
9. [Key Takeaways](#key-takeaways)

---

## E-Mail 1 - HR Attendance Task

**HR REPORT**

Sending you these tasks following up on our call.  
The HR dataset can be found in the file `attendance_data.csv`. Your tasks are divided into two parts: data cleaning and data analysis.

---

### Task 1: Data Cleaning

- Check for duplicates in the dataset and remove them.
- Standardize the date values to the format `YYYY-MM-DD` and extract the month name and day type from them.
- Remove any extra characters, such as special characters, from the employee ID values. Some IDs may contain a `@` character at the end, which can be cleaned and brought to a common format.
- Standardize the capitalization of names. Convert all names to title case, which means capitalizing the first letter of each word.
- Map the corresponding values in the status column with the given abbreviations:

| Full Form        | Abbreviation |
|------------------|--------------|
| Work From Office | WFO          |
| Work From Home   | WFH          |
| Birthday Leave   | BL           |
| Menstrual Leave  | ML           |
| Paid Leave       | PL           |
| Sick Leave       | SL           |
| Weekly Off       | WO           |

---

### Task 2: Ad Hoc Analysis

- What is the total count of distinct employee names within the dataset?
- Calculate the work-from-home (WFH %) percentage in the month of May.
- Determine which day of the week had the highest attendance percentage in the month of June.
- Find out the number of employees who had a WFH percentage greater than 10% in the month of April.

You are free to use any tool of your preference, such as Pandas, Excel, PowerBI, etc., to complete this task.  
Good luck with your task!

---

## Work and Response - Task 1 Data Cleaning

**Tool Used:** Power BI - Power Query

**Steps Applied:**

- **Remove Duplicates:** Identified and removed all duplicate rows from the dataset.
- **Standardize Dates:** Converted all date values to `YYYY-MM-DD` format and extracted `Month Name` and `Day Type` as new columns.
- **Clean Employee IDs:** Stripped trailing `@` and other special characters from employee ID values to bring them to a consistent format.
- **Title Case Names:** Applied title case transformation to all employee name values.
- **Map Status Abbreviations:** Replaced all full-form status values with their corresponding abbreviations using a mapping transformation in Power Query.

---

## Work and Response - Task 2 Analysis

**Tool Used:** Power BI - DAX Measures

---

### Question 1 - Total Count of Distinct Employees

```dax
-- Measure 1
Total Rows = COUNTROWS(attendance_data)

-- Measure 2
Count Distinct Employee Names = DISTINCTCOUNT(attendance_data[name])

-- Measure 3
Distinct Employee IDs = DISTINCTCOUNT('attendance_data'[employee_id])
```

**Result:** 99 distinct employees by name | 74 distinct employee IDs

---

### Question 2 - WFH Percentage in May

```dax
-- Measure 4
WFH Records May =
CALCULATE(
    COUNTROWS('attendance_data'),
    MONTH('attendance_data'[date]) = 5,
    'attendance_data'[status_abbreviations] = "WFH"
)

-- Measure 5
WFO Records May =
CALCULATE(
    COUNTROWS('attendance_data'),
    MONTH('attendance_data'[date]) = 5,
    'attendance_data'[status_abbreviations] = "WFO"
)

-- Measure 6
Total Present Days May = [WFH Records May] + [WFO Records May]

-- Measure 7
WFH % May =
DIVIDE(
    [WFH Records May],
    [Total Present Days May]
)
```

**Result:** WFH % for May = **11.49%**

---

### Question 3 - Highest Attendance Day in June

```dax
-- Measure 8
Present Records June =
CALCULATE(
    COUNTROWS('attendance_data'),
    'attendance_data'[Month Name] = "June",
    'attendance_data'[status_abbreviations] IN { "WFO", "WFH" }
)

-- Measure 9
Total Records June =
CALCULATE(
    COUNTROWS('attendance_data'),
    'attendance_data'[Month Name] = "June"
)

-- Measure 10
Attendance % June =
DIVIDE(
    [Present Records June],
    [Total Records June]
)
```

**Result:**

| Day       | Attendance Percentage |
|-----------|-----------------------|
| Tuesday   | 97.53% - Highest      |
| Wednesday | 95.43%                |
| Monday    | 93.41%                |
| Sunday    | 14.06% - Lowest       |

---

### Question 4 - Employees with WFH Greater Than 10% in April

```dax
-- Measure 11
WFH Records April =
CALCULATE(
    COUNTROWS('attendance_data'),
    'attendance_data'[status_abbreviations] = "WFH",
    FILTER(
        'attendance_data',
        MONTH('attendance_data'[date]) = 4
    )
)

-- Measure 12
WFO Records April =
CALCULATE(
    COUNTROWS('attendance_data'),
    MONTH('attendance_data'[date]) = 4,
    'attendance_data'[status_abbreviations] = "WFO"
)

-- Measure 13
WFH % April =
DIVIDE(
    [WFH Records April],
    [WFH Records April] + [WFO Records April]
)

-- Measure 14
Employees WFH Greater Than 10% April =
COUNTROWS(
    FILTER(
        VALUES('attendance_data'[name]),
        [WFH % April] > 0.10
    )
)
```

**Result:** 16 employees had a WFH percentage greater than 10% in April.

---

## Dashboard Summary

| Metric                                       | Value             |
|----------------------------------------------|-------------------|
| Total Records                                | 6,208             |
| Distinct Employees by Name                   | 99                |
| Distinct Employee IDs                        | 74                |
| WFH Percentage - May                         | 11.49%            |
| Employees with WFH Greater Than 10% - April  | 16                |
| Highest Attendance Day - June                | Tuesday at 97.53% |

---

## E-Mail 2 - Data Normalization Task

One of our client projects requires minor support. The dataset is currently in a de-normalized form, and we need your help to transform it into a normalized form.

Your task will involve creating proper fact and dimension tables based on the dataset, which can be found in the `fact_order_lines.csv` file.

---

## Work and Response - Data Normalization

**Tool Used:** Power BI - Power Query and Model View

### Objective

Transform the denormalized transactional dataset `fact_order_lines.csv` into a normalized star schema structure by separating it into appropriate fact and dimension tables.

---

### Understanding the Source Data

The original dataset was a single denormalized table containing all of the following:

| Category              | Columns                                             |
|-----------------------|-----------------------------------------------------|
| Order Information     | order_id, order_placement_date, mmm_yy, week_no     |
| Customer Information  | customer_id, customer_name, city                    |
| Product Information   | product_id, product_name, category                  |
| Quantitative Measures | order_qty, delivery_qty                             |

All attributes were stored in one table, resulting in redundancy and repeated descriptive information.

---

### Dimension Tables Created

**dim_customers**

- Columns retained: `customer_id`, `customer_name`, `city`
- Duplicates removed

**dim_products**

- Columns retained: `product_id`, `product_name`, `category`
- Duplicates removed

**dim_dates**

- Columns retained: `order_placement_date` renamed to `date`, `mmm_yy`, `week_no`
- Duplicates removed

---

### Fact Table

**fact_orders**

- Columns retained: `order_id`, `order_placement_date`, `customer_id`, `product_id`, `order_qty`, `delivery_qty`
- No duplicates removed - each row represents a unique transaction-level record

---

### Star Schema Relationships

```
dim_customers[customer_id]  --  1 to Many  -->  fact_orders[customer_id]
dim_products[product_id]    --  1 to Many  -->  fact_orders[product_id]
dim_dates[date]             --  1 to Many  -->  fact_orders[order_placement_date]
```

- Cardinality: One-to-Many
- Cross filter direction: Single

---

### Validation

```
row_count(dim_customers) + row_count(dim_dates) + row_count(dim_products) = 236
```

This confirmed that duplicates were correctly removed and dimension tables were properly structured.

---

## E-Mail 3 - SCRUM Training

As part of your internship, we will be covering the SCRUM methodology in software development. SCRUM is a widely-used agile methodology that helps teams collaborate effectively to deliver high-quality software products.

It is mandatory for you to complete the SCRUM Training Series available at https://scrumtrainingseries.com/

Good luck with your training!

---

## Work and Response - SCRUM Learnings

---

### Module 1 - Scrum in Small vs Large Organizations

**Why Scrum Works Well in Small Companies**

- The Product Owner directly shares business objectives with a clear and evolving product vision.
- Developers interact with customers directly, enabling faster learning and feedback.
- The team is cross-functional and handles analysis, architecture, design, coding, testing, integration, deployment, and UX.
- The team evolves into a self-managing unit with no rigid internal role distinctions.

**Problems When Scaling Scrum in Large Organizations**

| Problem                      | Impact                                                        |
|------------------------------|---------------------------------------------------------------|
| Fragmented team backlogs     | Hides global priorities, most valuable work may be missed     |
| Component teams              | Teams produce parts, not end-to-end customer features         |
| Velocity obsession           | Increased team output does not equal increased customer value |
| No direct customer contact   | Developers talk to intermediaries instead of real customers   |
| Overspecialization           | Teams specialize in narrow areas, skills become outdated      |

**What Real Scrum Emphasizes**

- Only three roles: Product Owner, Scrum Master, Developers
- One real Product Backlog containing the most important problems in one place
- Real Product Owner with authority to make major business decisions
- Scrum Masters remove barriers between teams
- The ultimate source of priorities remains the Product Owner

---

### Module 2 - Backlog Refinement

**Purpose**

Clarify and decompose higher-priority Product Backlog Items before Sprint Planning. Ideally done a couple of workdays before Sprint Planning so the Product Owner has time to revise priorities.

**Participants**

- Product Owner - owns product vision and makes final decisions on prioritization
- Development Team - collectively responsible for delivering potentially shippable increments
- Scrum Master - participates only as needed or requested

**PBI Quality Standard - INVEST**

| Letter | Meaning     |
|--------|-------------|
| I      | Independent |
| N      | Negotiable  |
| V      | Valuable    |
| E      | Estimable   |
| S      | Small       |
| T      | Testable    |

**Key Rules**

- High-priority PBIs should be no bigger than one quarter of a sprint
- No commitments are made during refinement - commitment happens in Sprint Planning
- The entire team must participate - refinement requires technical, testing, UI, and risk perspectives

---

### Module 3 - Sprint Planning

**Two Topics of Sprint Planning**

| Topic          | What Happens                                                                      |
|----------------|-----------------------------------------------------------------------------------|
| Topic 1 - What | Product Owner presents top priorities, team selects PBIs, Sprint Goal is agreed   |
| Topic 2 - How  | Development Team creates Sprint Backlog with tasks, design, and testing steps     |

**Sprint Goal**

- Provides focus and aligns all selected PBIs
- Prevents unrelated work from entering the sprint

**Definition of Done Check**

Before closing the meeting, the team confirms:

- Selected PBIs are achievable within the sprint
- Definition of Done will be met without compromise
- No overtime is required
- Sprint Goal is supported by all selected items

**Task Assignment**

Tasks are not pre-assigned during planning. Team members volunteer at the last responsible moment during sprint execution to encourage fluid collaboration.

---

### Module 4 - Daily Scrum

**Key Characteristics**

- Occurs every day at the same time and same place
- 15-minute time box
- Usually conducted standing
- Owned by the Development Team, not the Scrum Master

**Traditional Three Questions**

1. What did I do yesterday?
2. What will I do today?
3. What impediments are blocking me?

Note: Modern Scrum does not mandate these questions. Teams can design their own format.

**Agile Engineering Practices Covered**

- Test-Driven Development: Write a failing test, write minimal code to pass it, refactor, and repeat
- Pair Programming: Two developers work together on the same code for shared knowledge and better quality
- Continuous Integration: Automatically runs tests after each code change to detect failures quickly

**Handling Side Discussions**

If deeper discussion is needed, it is captured as a sidebar topic and discussed after the Daily Scrum to protect the 15-minute time box.

---

### Module 5 - Sprint Review

**Purpose**

The Sprint Review happens at the end of the sprint to inspect the product increment, demonstrate completed work, gather stakeholder feedback, and adapt the Product Backlog.

**Definition of Done**

A PBI is considered done only if it fully meets the Definition of Done.

- Almost done does not count
- 80% done is not accepted
- Items must be fully tested and integrated before being declared done

**Stakeholder Feedback**

Stakeholders may suggest improvements or additional features during the review. All feedback becomes new Product Backlog Items prioritized by the Product Owner.

**Key Rule**

Only the Product Owner prioritizes work. No feature promises should be made by sales or other parties without the Product Owner's involvement.

---

## Key Takeaways

- Data cleaning is foundational. Dirty data leads to incorrect insights regardless of the analysis method used.
- Power BI DAX measures enable powerful and reusable analytical logic that can be maintained and updated easily.
- Star schema normalization improves model scalability, reduces redundancy, and improves query performance.
- Scrum's value comes from eliminating process overhead, empowering self-management, and maintaining focus on the whole product rather than maximizing individual team velocity in isolation.
- A real Product Owner with full authority and a single unified backlog is the most critical element of a successful Scrum implementation.

---

*Deepak Biswas - Week 1 Internship Report*
