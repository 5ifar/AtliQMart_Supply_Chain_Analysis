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
- [Conclusion]()

---

## [AtliQ Mart: Supply Chain Analysis Live Dashboard Link]()

---

## Project Objective:

AtliQ Mart is a growing FMCG manufacturer headquartered in Gujarat, India. It is currently operational in three cities Surat, Ahmedabad and Vadodara. They want to expand to other metros/Tier 1 cities in the next 2 years.

AtliQ Mart is currently facing a problem where a few key customers did not extend their annual contracts due to service issues. It is speculated that some of the essential products were either not delivered on time or not delivered in full over a continued period, which could have resulted in bad customer service. Management wants to fix this issue before expanding to other cities and requested their supply chain analytics team to track the ’On time’ and ‘In Full’ delivery service level for all the customers daily basis so that they can respond swiftly to these issues.

The Supply Chain team decided to use a standard approach to measure the service level in which they will measure ‘On-time delivery (OT) %’, ‘In-full delivery (IF) %’, and OnTime in full (OTIF) %’ of the customer orders daily basis against the target service level set for each customer.

Other resources Provided: Relevant Business Knowledge & Metadata. These can be refered from the Assets folder.

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
The dataset contains 6 tables in total, all as CSV files namely:
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
