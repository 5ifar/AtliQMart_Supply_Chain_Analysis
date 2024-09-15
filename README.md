# <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Assets/AtliQ%20Mart%20Logo.png" width="4%" height="4%"> AtliQ Mart: Supply Chain Analysis

This repository serves as my documentation for the AtliQ Mart: Supply Chain Analysis - Power BI Project.
It was created as part of [Resume Project Challenge 2: Generate Insights to Solve a Supply Chain Issue in the FMCG domain](https://codebasics.io/challenge/codebasics-resume-project-challenge/5) by [Codebasics](https://codebasics.io/).

The entire project has been implemented using Microsoft Power BI Desktop 2.132.1053.0 and published on Microsoft Power BI Service.

---

## Contents:
Please find the sectional links for the project below:
- [Supply Chain Analysis Live Dashboard Link]()
- [Project Objective]()
- [Tools used & Methodologies implemented]()
- [About the Dataset]()
  - [Data Sources]()
  - [Data Integrity]()
- [Data Model]()
- [Project Implementation]()
- [Supply Chain Analysis Dashboard Overview]()
- [Data-driven Insights]()
- [Conclusion]()

---

## [AtliQ Mart: Supply Chain Analysis Live Dashboard Link]()

---

## Project Objective:

AtliQ Mart is a growing FMCG manufacturer headquartered in Gujarat, India. It is currently operational in three cities Surat, Ahmedabad and Vadodara. They want to expand to other metros/Tier 1 cities in the next 2 years.

AtliQ Mart is currently facing a problem where a few key customers did not extend their annual contracts due to service issues. It is speculated that some of the essential products were either not delivered on time or not delivered in full over a continued period, which could have resulted in bad customer service. Management wants to fix this issue before expanding to other cities and requested their supply chain analytics team to track the ’On time’ and ‘In Full’ delivery service level for all the customers daily basis so that they can respond swiftly to these issues.

The Supply Chain team decided to use a standard approach to measure the service level in which they will measure ‘On-time delivery (OT) %’, ‘In-full delivery (IF) %’, and OnTime in full (OTIF) %’ of the customer orders daily basis against the target service level set for each customer.

Other resources Provided: Relevant Business Knowledge & Metadata. These files can be refered from the [Assets](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/tree/main/Assets) folder.

---

## Tools used:
1. Microsoft Power BI: for Data ETL, Data Modelling, Data Visualization & Dashboarding
2. GitHub - for Documentation

## Skills & Methodologies implemented:
1. Data Cleaning: **Power Query**
2. Data Manipulation: **DAX Measures & Columns, Parameters**
3. Data Modelling
4. Data Visualization: **Conditional Formatting**
5. Dashboarding: **Filters, Custom Icon Buttons, Slicers, Bookmarks, Page Navigation**
6. Report Publishing: **PBI Service and Report Optimization**
7. Documentation

---

## About the Dataset:

### Data Sources:
The [Dataset](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/tree/main/Dataset) contains 6 tables in total, all as CSV files namely:
- dim_customer.csv: 35 records | 3 columns
- dim_date.csv: 183 records | 3 columns
- dim_product.csv: 18 records | 3 columns
- dim_targets_orders.csv: 35 records | 4 columns
- fact_order_lines.csv: 57,096 records | 11 columns
- fact_orders_aggregate.csv: 31,729 records | 6 columns

## Data Integrity:
ROCCC Evaluation:
- Reliability: MED - The raw dataset is created and updated by Codebasics. It has total 6 files. All of them were utilized in the analysis.
- Originality: HIGH - First party provider (Codebasics)
- Comprehensiveness: MED - Total 6 Files with a total of around 90 Thousand records were provided. Dataset contains multiple dimension parameters for Customers & Products as well as comprehensive order transaction data.
- Current: MED - Dataset has FY 2022 data i.e almost 2 years old. So its not very relevant. Any trends observed and insights gained need to be comprehended as a general (not FY-specific) trend.
- Citation: HIGH - Official citation/reference available.

---

## Data Model:
<div align="center"> <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Data%20Model/Supply%20Chain%20Analytics%20Data%20Model.PNG" width="100%" height="100%"> </div>

---

## Project Implementation:
Please find the documentation links for the project phase-wise implementation below:
- [Phase 1: ETL with Power Query](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#phase-1-etl-with-power-query)
- [Phase 2: Data Modelling](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#phase-2-data-modelling)
- [Phase 3: Measures & Calculated Columns](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#phase-3-measures--calculated-columns)
  - [Calculated Columns](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#calculated-columns)
  - [Measures](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#measures)
- [Phase 4: Dashboard Design](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#phase-4-dashboard-design)
  - [Color Pallet](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#color-pallet)
  - [I. Glance at KPIs](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#i-glance-at-kpis)
  - [II. Glance at Cities](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#ii-glance-at-cities)
  - [III. Gaze at KPI Trends](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#iii-gaze-at-kpi-trends)
  - [IV. Gaze at Customers](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#iv-gaze-at-customers)
  - [V. Gaze at Products](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#v-gaze-at-products)
  - [VI. Gaze at Customer Lead Times](https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Project%20Implementation/Documentation.md#vi-gaze-at-customer-lead-times)

---

## Data-driven Insights:

### 1. Product Category

- Dairy dominates product category orders with 2 times the total orders of Food and Beverages categories combined.

<div align="center"> <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Assets/Insight%20Images/Insight%201%20-%20Product%20Category.PNG" width="50%" height="50%"> </div>

### 2. KPI Performance

- Average OT %, IF % & OTIF % metric levels are alarmingly lower than respective targets.

<div align="center"> <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Assets/Insight%20Images/Insight%202%20-%20KPI%20Performance.PNG" width="50%" height="50%"> </div>

### 3. Top Customers

- Vijay Stores, Rel Fresh, Lotus mart, Propel Mart and Acclaimed Stores are the key customers with individual order lines averaging almost 1.5 times rest of the customers.

<div align="center"> <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Assets/Insight%20Images/Insight%203%20-%20Top%20Customers.PNG" width="50%" height="50%"> </div>

### 4. Monthly KPI Trends

- July 2022 was the only month when all 3 KPIs - OT %, IF % & OTIF % showed improvement compared to previous month while still performing on average 29% lower than respective targets.

<div align="center"> <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Assets/Insight%20Images/Insight%204.1%20-%20Monthly%20KPI%20Trends.PNG" width="50%" height="50%"> </div>
<div align="center"> <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Assets/Insight%20Images/Insight%204.2%20-%20Monthly%20KPI%20Trends.PNG" width="50%" height="50%"> </div>







