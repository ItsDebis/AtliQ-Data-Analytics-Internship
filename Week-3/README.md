# Internship Week 3 Summary

**Intern:** Deepak Biswas
**Week:** Week 3
**Domain:** Data Analytics

---

## Table of Contents

1. [Task 1 — Benchmark Creation & Data Understanding](#task-1--benchmark-creation--data-understanding)
2. [Task 2 — Dashboard Mockup Design](#task-2--dashboard-mockup-design)
3. [Key Takeaways](#key-takeaways)

---

## Task 1 — Benchmark Creation & Data Understanding

**Tool Used:** Microsoft Excel

### Incoming Task Email

> **Subject:** Feature List
>
> The task is to create a feature list based on the email provided below from the client and a sample feature list. The email contains project requirements which need to be analyzed and included in the feature list. The sample feature list available in the download section serves as a reference for the created feature list. The created feature list must capture all the mentioned features and include relevant details.
>
> **Pilot Project requirement**
>
> "Hemanand,
>
> I hope this email finds you well. I'm summarizing the requirements for the pilot project as discussed in the last call.
>
> To begin, we would like to focus on understanding the number of customers we have and the total revenue we are generating. It would also be beneficial to track the daily revenue growth rate and daily customer growth rate to monitor our progress.
>
> Monitoring changes in policies on a month-over-month basis is also important to identify trends and areas for improvement. It would be helpful to segment our customer base by age group and analyze revenue and customer numbers by city and age group.
>
> To analyze trends in customer and revenue growth over time, it would be great to create a switch between revenue trend graphs and customer trend graphs. Additionally, using filters to analyze sales mode, age group, city, month, and policy ID would make the analysis more efficient.
>
> It would be valuable to have a separate page for sales mode analysis to better understand our customer demographics. We can calculate total customers and total revenue split percentages by sales mode and analyze the trend of sales mode over the month.
>
> Having a separate page for age group analysis would also be helpful to understand the impact of age groups on our business. We can analyze age group data to understand expected settlement, sales mode, and policy preference, which will help us make informed business decisions.
>
> We believe that this pilot project will help us gain valuable insights and build confidence in our collaboration. I would be happy to answer any questions that you may have.
>
> I'm also attaching the data & metadata for your reference."

---

### Objective

The objective of this task was to convert unstructured client requirements into a structured and reusable feature list.

Before building any feature list, the first step was to thoroughly understand the data. The metadata provided described the structure and meaning of every table and column in the dataset. This understanding formed the foundation for identifying which features were technically feasible and how each business requirement mapped to the available data.

The **benchmark** in this context refers to the feature list itself — a structured document that captures all agreed-upon requirements, priorities, and business intent before dashboard development begins. It acts as a reference point throughout the project to ensure nothing is missed and all work stays aligned with client expectations.

---

### Data Understanding — Metadata Overview

The dataset consists of two types of tables: **dimension tables** (providing context) and **fact tables** (containing transactional data).

**Dimension Tables**

| Table          | Key Columns                              | Purpose                                     |
|----------------|------------------------------------------|---------------------------------------------|
| dim_customer   | customer_code, dob, city                 | Customer segmentation by age and location   |
| dim_date       | date, mmm_yy, day_type, week_no          | Time-based analysis (daily trends, MoM)     |
| dim_policies   | policy_id, base_cover, base_premium_amt  | Policy-level insights and product analysis  |

**Fact Tables**

| Table              | Key Columns                                              | Purpose                                        |
|--------------------|----------------------------------------------------------|------------------------------------------------|
| fact_premiums      | date, customer_code, policy_id, sales_mode, final_premium_amt | Revenue, customer count, and sales analysis |
| fact_settlements   | age, settlement%                                         | Risk and settlement analysis by age            |

---

### Business Requirement Breakdown

The client email was carefully read and each requirement was mapped to a specific feature category:

**Core KPIs** — Total customers, total revenue, daily customer growth, daily revenue growth

**Time-Based Analysis** — Month-over-month policy change, revenue trends, customer trends over time

**Segmentation Analysis** — Age group segmentation (18–24, 25–30, 31–40, 41–50, 51–65, 65+), city-wise analysis, combined city and age group analysis

**Interactivity** — Filters by sales mode, age group, city, month, and policy ID; switch between revenue and customer trend graphs

**Deep-Dive Pages** — A dedicated sales mode analysis page and a dedicated age group analysis page

---

### Feature List

The feature list was structured with four components for each entry: a sequential number (Sno), the feature description, a business comment explaining its value, and a priority level (High / Low).

| Sno | Feature | Comments | Priority |
|-----|---------|----------|----------|
| 1 | Show total customers, total revenue, daily revenue growth, daily customer growth as key metrics | To monitor all the crucial metrics | High |
| 2 | Month over month change% on key metrics | Tracking the month-over-month change in policies on key metrics can offer valuable insights into a company's performance trends | High |
| 3 | Segment customers based on their age groups: 18–24, 25–30, 31–40, 41–50, 51–65, and 65+ | To categorise customers into different age groups to better understand behavior and preferences | High |
| 4 | Total revenue split by age group, city | To identify most profitable customer demographics and geographical areas | High |
| 5 | Total customers split by age group, city | To understand customer locations and age groups so we can customise products and marketing strategies | High |
| 6 | Customers, daily customers growth trend by month | To understand customer growth trends over time and spot changes in customer behavior to adjust marketing efforts accordingly | Low |
| 7 | Revenue, daily revenue growth trend by month | To track business financial performance and growth trends over time and spot any changes or fluctuations in revenue | Low |
| 8 | Create a switch between revenue trend graph and customer trend graph | Switching between revenue trend and customer trend graphs enhances user experience by providing a more customised and efficient way to view data | Low |
| 9 | Filters on sales mode, age group, city, month, policy ID | Provides users with the ability to filter and sort data according to specific parameters, allowing for more targeted and efficient analysis | High |
| 10 | Separate page for sales mode analysis | A dedicated page for sales mode analysis | High |
| 11 | Total customers split percentage by sales mode | Provides insights into which modes are most effective in attracting and retaining customers | High |
| 12 | Total revenue split percentage by sales mode | Provides insights into which modes are most effective in generating revenue for the business | High |
| 13 | Trend of sales mode over month | To understand the trend of sales mode over the month | Low |
| 14 | Separate page for age group analysis | A dedicated page for age group analysis | High |
| 15 | Age group vs expected settlement | To know what the expected annual settlement is | High |
| 16 | Age group vs sales mode | To understand what sales mode people prefer by age group | Low |
| 17 | Age group vs policy preference | To understand what policy people buy by age group | High |

![Feature List - Benchmark Document](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_3_T_1_image_1.png)

---

## Task 2 — Dashboard Mockup Design

**Tool Used:** Figma / Wireframe Tool

### Incoming Task Email

> **Subject:** Dashboard Creation Task
>
> "I saw the response from Mathew and you did a good job there. As a next step, you will be creating a dashboard, and I want to give you some quick tips.
>
> - Ensure that the dashboard you are creating meets Mathew's requirements.
> - Create a DAX metrics list so that you have clarity before building the dashboard.
> - Create documentation for Mathew so that they won't have issues in understanding the dashboard.
>
> I have attached a sample DAX metrics list and sample documentation from previous projects that should help guide you in the right direction. Study these resources and use them as a reference throughout your work.
>
> In case you need help, feel free to contact your seniors."

---

### Objective

Before building the actual dashboard in Power BI, a mockup was designed to define the visual layout, page structure, and placement of KPIs and charts. The mockup serves as a blueprint that aligns the development work with the client's expectations and avoids rework during later stages.

The mockup translates the feature list into a visual format — showing exactly what the client will see once the dashboard is built, without requiring any data connections or calculations at this stage.

---

### Mockup Design Approach

The dashboard was planned across multiple pages to keep the analysis focused and navigable:

**Page 1 — Overview / Home Page**

This page presents all core KPIs in a single view. It includes total customers, total revenue, daily revenue growth, and daily customer growth. Month-over-month change percentages are displayed alongside each metric. A toggle switch allows the user to switch between the revenue trend graph and the customer trend graph. Filters for sales mode, age group, city, month, and policy ID are placed consistently at the top or side panel.

**Page 2 — Sales Mode Analysis**

This page is dedicated to understanding how different sales channels (Offline-Agent, Offline-Direct, Online-App, Online-Website) contribute to the business. It shows customer count and revenue split as percentages by sales mode, along with a monthly trend line for each mode.

**Page 3 — Age Group Analysis**

This page focuses on customer demographics and risk. It includes a visualisation of expected settlement percentage by age group, a breakdown of sales mode preference by age group, and a chart showing which policies different age groups prefer.

---

### Mockup Previews

![Dashboard Mockup - Overview Page Layout](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_3_T_2_image_2.png)

![Dashboard Mockup - Sales Mode Analysis Page](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_3_T_2_image_3.png)

![Dashboard Mockup - Age Group Analysis Page](https://raw.githubusercontent.com/ItsDebis/AtliQ-Data-Analytics-Internship/main/Assets/Images/Week_3_T_2_image_4.png)

---

## Key Takeaways

- Reading metadata carefully before any development work is essential — it reveals the structure of the data and makes requirement mapping significantly more accurate.
- A well-structured feature list acts as a project roadmap, ensuring that all client requirements are captured before development begins and reducing the risk of missed features or scope creep.
- Assigning priority levels (High / Low) to features helps teams focus on what matters most and plan development in a logical sequence.
- A mockup bridges the gap between business requirements and technical execution — it gives stakeholders a concrete visual to review and approve before any data work begins.
- Breaking a dashboard into focused pages (Overview, Sales Mode, Age Group) improves usability and makes it easier for end users to navigate and extract insights.
- Understanding the difference between dimension tables (context) and fact tables (transactions) is fundamental to building a correct and reliable data model.

---

*Deepak Biswas — Week 3 Internship Report*
