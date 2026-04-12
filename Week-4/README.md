# Shield Insurance — Power BI Dashboard | AtliQ Technologies Internship

---

## Project Brief — Email from Mentor

> **From:** Hem, Head of Data Analytics | AtliQ Technologies Pvt. Ltd.
> **To:** Deepak Biswas
>
> I saw the response from Mathew and you did a good job there. As a next step, you will be creating a dashboard, and I want to give you some quick tips.
>
> - Ensure that the dashboard you are creating meets Mathew's requirements.
> - Create a **DAX metrics list** so that you have clarity before building the dashboard.
> - Create **documentation** for Mathew so that they won't have issues in understanding the dashboard.
>
> I have attached a sample DAX metrics list and sample documentation from previous projects that should help guide you in the right direction. Study these resources and use them as a reference throughout your work.
>
> In case you need help, feel free to contact your seniors.
>
> Best regards,
> **Hem**
> Head of Data Analytics | AtliQ Technologies Pvt. Ltd.
> [www.atliq.com](http://www.atliq.com)

---

## Project Title

**Shield Insurance — Business Intelligence Dashboard (Pilot Project)**
Built as part of the **Data Analytics Internship at AtliQ Technologies**

---

## Objective / Problem Statement

Shield Insurance Company provides reliable and comprehensive insurance plans for individuals and businesses, ensuring protection from various risks. To improve data-driven decision-making, Shield Insurance required a **Power BI dashboard** that delivers actionable insights into key performance metrics.

As a **Proof of Concept**, Shield Insurance commissioned AtliQ Technologies to deliver a **Pilot Project in Power BI** — demonstrating the capability to meet their specific analytical needs before committing to a full-scale engagement.

---

## Business Questions to Answer

| # | Requirement |
|---|-------------|
| 1 | Show total customers, total revenue, daily revenue growth (DRG), daily customer growth (DCG) as key metrics |
| 2 | Month-over-month change % on key metrics |
| 3 | Segment customers by age groups: 18-24, 25-30, 31-40, 41-50, 51-65, 65+ |
| 4 | Total revenue split by age group and city |
| 5 | Total customers split by age group and city |
| 6 | Customer and daily customer growth trend by month |
| 7 | Revenue and daily revenue growth trend by month |
| 8 | Toggle switch between revenue trend graph and customer trend graph |
| 9 | Filters on sales mode, age group, city, month, and policy ID |
| 10 | Dedicated page for sales mode analysis |
| 11 | Total customers split % by sales mode |
| 12 | Total revenue split % by sales mode |
| 13 | Trend of sales mode over month |
| 14 | Dedicated page for age group analysis |
| 15 | Age group vs. expected settlement |
| 16 | Age group vs. sales mode |
| 17 | Age group vs. policy preference |

---

## Tools & Technologies Used

| Tool | Purpose |
|------|---------|
| **Microsoft Excel + Power Query** | Data cleaning and transformation |
| **Power BI** | Data modeling, visualization, and dashboard creation |
| **DAX (Data Analysis Expressions)** | Custom measures and calculated columns |
| **DAX Studio** | DAX query optimization and testing |

---

## Approach — Step by Step

### 1. ASK — Define the Problem

Analyzed the client's requirements from the pilot project brief shared by Mathew (Business Analyst, Shield Insurance). Identified 17 key features across three dashboard views: **Overview**, **Sales Mode Analysis**, and **Age Group Analysis**.

---

### 2. PREPARE — Understand the Data

The dataset consists of **5 CSV files**:

| File | Description |
|------|-------------|
| `dim_customer.csv` | Customer details — `customer_code`, `dob`, `city` |
| `dim_date.csv` | Date hierarchy — `date`, `mmm_yy`, `day_type`, `week_no` |
| `dim_policies.csv` | Policy details — `policy_id`, `base_cover`, `base_premium_amt` |
| `fact_premiums.csv` | Policy sales — `date`, `customer_code`, `policy_id`, `sales_mode`, `final_premium_amt` |
| `fact_settlements.csv` | Settlement data — `age`, `settlement%` |

---

### 3. PROCESS — Data Cleaning & Transformation

Performed using **Microsoft Excel and Power Query**:

- Removed empty columns from `fact_settlements`
- Split the `sales_mode` column in `fact_premiums` by delimiter `'-'` and renamed as:
  - **Mode: Online/Offline**
  - **Mode: Through Medium**
- Verified and corrected all data types (e.g., `Settlement %` column)

---

### 4. ANALYZE — Data Modeling & DAX

#### Data Model

Built a clean **Star Schema** with fact and dimension tables properly related in Power BI.

![Data Model](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_1.png)

---

#### Calculated Columns

**`dim_customer` table:**
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

**`dim_date` table:**
```dax
Month = FORMAT(dim_date[date].[Date], "MMM")

SortedBy = SWITCH(TRUE(),
    dim_date[Month]="Nov", 1,
    dim_date[Month]="Dec", 2,
    dim_date[Month]="Jan", 3,
    dim_date[Month]="Feb", 4,
    dim_date[Month]="Mar", 5, 6)
```

---

#### Key DAX Measures

```dax
-- Core Metrics
Total Customers    = COUNT(dim_customer[customer_code])
Total Revenue      = SUM(fact_premiums[final_premium_amt(INR)])

-- Daily Growth Metrics
DRG  = VAR _tot_rev = [Total Revenue]
       VAR _date    = DISTINCTCOUNT(dim_date[date])
       RETURN DIVIDE(_tot_rev, _date, 0)

DCG  = VAR _tot_cus = [Total Customers]
       VAR _date    = DISTINCTCOUNT(dim_date[date])
       RETURN DIVIDE(_tot_cus, _date, 0)

-- Month-over-Month Comparisons
LM Customers   = CALCULATE([Total Customers], PREVIOUSMONTH(dim_date[date]))
LM Revenue     = CALCULATE([Total Revenue],   PREVIOUSMONTH(dim_date[date]))
LM DCG         = CALCULATE([DCG],             PREVIOUSMONTH(dim_date[date]))
LM DRG         = CALCULATE([DRG],             PREVIOUSMONTH(dim_date[date]))

-- MoM Change %
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

### 5. SHARE — Dashboard Views

#### Overview Page

![Overview Dashboard](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_2.png)

---

#### Sales Mode Analysis Page

![Sales Mode Analysis](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_3.png)

---

#### Age Group Analysis Page

![Age Group Analysis](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_4.png)

---

#### Documentation / DAX Metrics List

![DAX Metrics and Documentation](https://github.com/ItsDebis/AtliQ-Data-Analytics-Internship/blob/main/Assets/Images/Week_4_T_1_image_5.png)

---

## Key Insights & Findings

| # | Insight |
|---|---------|
| 1 | **Total Customers:** Shield Insurance serves **26,841 customers** across all cities |
| 2 | **Total Revenue:** The company generated **Rs. 989.25 Million** in premium revenue |
| 3 | **Top City:** Delhi leads with **11,007 customers** and revenue of **Rs. 401.57 Million** |
| 4 | **Top Age Group:** The 31-40 age group is the largest segment with **11,171 customers** contributing **Rs. 343.76 Million** in revenue |
| 5 | **Best Month:** March 2023 recorded exceptional growth — **+85% revenue** and **+82% customer** increase |
| 6 | **Sharpest Decline:** April 2023 saw a steep drop of **-41.73% revenue** and **-41.41% customers** |
| 7 | **Dominant Sales Channel:** The Offline Agent channel accounts for **55.41% of customers** and **55.67% of revenue** |
| 8 | **Revenue Balance:** Revenue across all sales modes is fairly balanced, ranging from **12.60% to 16.27%** |
| 9 | **Top Policy by Customers:** `POL4321HEL` — chosen by **4,434 customers** |
| 10 | **Top Policy by Revenue:** `POL2005HEL` — generated **Rs. 324.26 Million** in revenue |

---

## Recommendations

- **Invest in Digital Channels** — While offline agents dominate, scaling digital acquisition (Online-App, Online-Website) can diversify revenue streams and reduce dependency on a single channel.

- **Target the 31-40 Age Group** — This segment drives the highest customer count and revenue. Tailored policy offerings and targeted marketing for this group can further strengthen retention.

- **Investigate April 2023 Drop** — The sharp decline in both customers and revenue in April warrants a root cause analysis to prevent similar drops and ensure consistent growth.

- **Promote High-Performing Policies** — Expand marketing around `POL4321HEL` (most popular) and `POL2005HEL` (highest revenue) to maximize their reach and conversion rates.

---

## Conclusion

This pilot project successfully demonstrated AtliQ Technologies' capability to deliver a comprehensive, insight-driven Power BI dashboard tailored to Shield Insurance's business needs. The dashboard covers all 17 client requirements across three dedicated views — **Overview**, **Sales Mode Analysis**, and **Age Group Analysis** — and is fully equipped with dynamic filters, trend toggles, DAX-powered KPIs, and clear documentation.

The insights derived provide Shield Insurance with a strong foundation for data-driven strategic decisions, making a compelling case for a full-scale collaboration.

---

<div align="center">

**Deepak Biswas** | Data Analyst Intern
**AtliQ Technologies** | Remote Internship

</div>
