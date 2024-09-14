# <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Assets/AtliQ%20Mart%20Logo.png" width="4%" height="4%"> AtliQ Mart: Supply Chain Analysis Project Phase-wise Implementation

---

## Phases of Implementation


---

## Phase 1: ETL with Power Query

All the 6 CSV Source Files (dim_customer.csv, dim_date.csv, dim_product.csv, dim_order_target.csv, fact_order_line.csv and fact_order_aggregate.csv) were loaded to the Power Query Editor where the following cleaning and transformations were applied and finally loaded to the PBI Frontend.

File: dim_customer: Changed customer_id datatype to Text to avoid unintentional aggregation.

File: dim_date: Renamed: mmm_yy → month & week_no → week

File: dim_product: Changed product_id datatype to Text to avoid unintentional aggregation. Set category Column to Proper Case.

File: dim_order_target: Changed customer_id datatype to Text to avoid unintentional aggregation. Divide ontime_target%, infull_target% and otif_target% columns by 100 to adjust values as percentages.

File: fact_order_line: Changed customer_id & product_id datatype to Text to avoid unintentional aggregation. Renamed: order_placement_date → order_date.

File: fact_order_aggregate: Changed customer_id datatype to Text to avoid unintentional aggregation. Renamed: order_placement_date → order_date.

---

## Phase 2: Data Modelling
The 2 Fact Tables and 4 Dimension Tables were connected in a Star Schema. The configured PKey → FKey relationships are as follows:
|Primary Key (1)||Foreign Key (*)|
|-|-|-|
|product_id (dim_product)|→|product_id (fact_order_line)|
|customer_id (dim_customer)|→|customer_id (fact_order_line)|
|customer_id (dim_customer)|→|customer_id (fact_order_aggregate)|
|customer_id (dim_customer)|→|customer_id (dim_order_target)|
|date (dim_date)|→|order_date (fact_order_line)|
|date (dim_date)|→|order_date (fact_order_aggregate)|

<div align="center"> <img src="https://github.com/5ifar/AtliQMart_Supply_Chain_Analysis/blob/main/Data%20Model/Supply%20Chain%20Analytics%20Data%20Model.PNG" width="50%" height="50%"> </div>

---

