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

## Phase 3: Measures & Calculated Columns

- In Data view: Set dim_date[month] column as custom format “Mmm yy”.

### Calculated Columns:

- In fact_order_line table: 
`delivery_delay = DATEDIFF([agreed_delivery_date], [actual_delivery_date], DAY)`
This column is formatted as Whole number and calculates the difference in interval of days between agreed and actual delivery dates.
- In fact_order_line table: 
`qty_variance = [order_qty] - [delivery_qty]`
This column calculates the difference between product quantity that was ordered and quantity that was delivered.
- In dim_product table:
`product_type` = product_name data extracted after skipping 1 space delimiter.
This column denotes the type of product.

### Measures:

- Base Measures:
    - Total Order Lines: Count of all order lines. (Reference table: fact_order_line)
    `Total Order Lines = COUNT(fact_order_line[order_id])`
    - Total Orders: Distinct Count of all orders. (Reference table: fact_order_aggregate)
    `Total Orders = DISTINCTCOUNT(fact_order_aggregate[order_id])`
- IF Measures:
    - IF Orders: Total Orders shipped In Full. (Reference table: fact_order_aggregate)
    `IF Orders = CALCULATE([Total Orders], fact_order_aggregate[in_full] = 1)`
    - IF %: Percentage of Total Orders that were shipped In Full.
    `IF % = DIVIDE([IF Orders], [Total Orders], 0)`
    - IF % Target: Average of In Full Target % across customers.
    `IF % Target = AVERAGE(dim_order_target[infull_target%])`
    - IF % Variance: Difference between In Full % - Actual and Target values.
    `IF % Variance = [IF %] - [IF % Target]`
- OT Measures:
    - OT Orders: Total Orders shipped On Time. (Reference table: fact_order_aggregate)
    `OT Orders = CALCULATE([Total Orders], fact_order_aggregate[on_time] = 1)`
    - OT %: Percentage of Total Orders that were shipped On Time.
    `OT % = DIVIDE([OT Orders], [Total Orders], 0)`
    - OT % Target: Average of On Time Target % across customers.
    `OT % Target = AVERAGE(dim_order_target[ontime_target%])`
    - OT % Variance: Difference between On Time % - Actual and Target values.
    `OT % Variance = [OT %] - [OT % Target]`
- OTIF Measures:
    - OTIF Orders: Total Orders shipped On Time and In Full. (Reference table: fact_order_aggregate)
    `OTIF Orders = CALCULATE([Total Orders], fact_order_aggregate[otif] = 1)`
    - OTIF %: Percentage of Total Orders that were shipped On Time and In Full.
    `OTIF % = DIVIDE([OTIF Orders], [Total Orders], 0)`
    - OTIF % Target: Average of On Time In Full Target % across customers.
    `OTIF % Target = AVERAGE(dim_order_target[otif_target%])`
    - OTIF % Variance: Difference between On Time In Full % - Actual and Target values.
    `OTIF % Variance = [OTIF %] - [OTIF % Target]`
- FR Measures:
    - Total Ordered Qty: Sum of total product qty ordered. (Reference table: fact_order_line)
    `Total Ordered Qty = SUM(fact_order_line[order_qty])`
    - Total Delivered Qty: Sum of total product qty delivered. (Reference table: fact_order_line)
    `Total Delivered Qty = SUM(fact_order_line[delivery_qty])`
    - IF Order Lines: Total Order Lines shipped In Full. (Reference table: fact_order_line)
    `IF Order Lines = CALCULATE([Total Order Lines], fact_order_line[In Full] = 1)`
    - LiFR %: Percentage of Total Order Lines that were shipped In Full.
    `LiFR % = DIVIDE([IF Order Lines], [Total Order Lines], 0)`
    - VoFR%: Percentage of Total Ordered Qty that was delivered.
    `VoFR % = DIVIDE([Total Delivered Qty], [Total Ordered Qty], 0)`
- Delay Measures:
    - 1D Delay Orders: Distinct Count of all orders with 1 Day of Delivery Delay. (Reference table: fact_order_line)
    `1D Delay Orders = CALCULATE(DISTINCTCOUNT(fact_order_line[order_id]), fact_order_line[delivery_delay] = 1)`
    - 2D Delay Orders: Distinct Count of all orders with 2 Days of Delivery Delay. (Reference table: fact_order_line)
    `2D Delay Orders = CALCULATE(DISTINCTCOUNT(fact_order_line[order_id]), fact_order_line[delivery_delay] = 2)`
    - 3D Delay Orders: Distinct Count of all orders with 3 Days of Delivery Delay. (Reference table: fact_order_line)
    `3D Delay Orders = CALCULATE(DISTINCTCOUNT(fact_order_line[order_id]), fact_order_line[delivery_delay] = 3)`
    - ADDD: Average Days of Delayed Delivery for all orders. (Reference table: fact_order_line)
    `ADDD = CALCULATE(AVERAGE(fact_order_line[delivery_delay]), fact_order_line[delivery_delay] > 0)`
    - Delay Orders: Total orders with Delayed Delivery.
    `Delay Orders = [1D Delay Orders] + [2D Delay Orders] + [3D Delay Orders]`
- Dynamic Input Measures:
    - All KPI Targets: Dynamic Target input based on metric selected from All KPIs Parameter.
    `All KPI Targets = SWITCH(SELECTEDVALUE('All KPIs Parameter'[All KPIs Parameter Order]), 0, [OT % Target], 1, [IF % Target], 2, [OTIF % Target])`
    - All KPI Variances: Dynamic Variance input based on metric selected from All KPIs Parameter.
    `All KPI Variances = SWITCH(SELECTEDVALUE('All KPIs Parameter'[All KPIs Parameter Order]), 0, [OT % Variance], 1, [IF % Variance], 2, [OTIF % Variance])`
    - Base KPI Targets: Dynamic Target input based on metric selected from Base KPIs Parameter.
    `Base KPI Targets = SWITCH(SELECTEDVALUE('Base KPIs Parameter'[Base KPIs Parameter Order]), 0, [OT % Target], 1, [IF % Target], 2, [OTIF % Target])`
    - Base KPI Variances: Dynamic Variance input based on metric selected from Base KPIs Parameter.
    `Base KPI Variances = SWITCH(SELECTEDVALUE('Base KPIs Parameter'[Base KPIs Parameter Order]), 0, [OT % Variance], 1, [IF % Variance], 2, [OTIF % Variance])`
- Visual Title Measures:
    - `KPIs Performance Title = SWITCH(SELECTEDVALUE('All KPIs Parameter'[All KPIs Parameter Order]), 0, "OT %", 1, "IF %", 2, "OTIF %", 3, "LiFR %", 4, "VoFR %") & " Trend over Time"`
    - `KPIs Breakdown Title = SWITCH(SELECTEDVALUE('All KPIs Parameter'[All KPIs Parameter Order]), 0, "OT %", 1, "IF %", 2, "OTIF %", 3, "LiFR %", 4, "VoFR %") & " Breakdown Trend by " & SWITCH(SELECTEDVALUE('CC Parameter'[CC Parameter Order]), 0, "City", 1, "Category")`
    - `KPIs vs City Title = SWITCH(SELECTEDVALUE('Base KPIs Parameter'[Base KPIs Parameter Order]), 0, "OT %", 1, "IF %", 2, "OTIF %") & " vs Target by City"`
    - `CC Delivery Delay Title = "Delivery Delay Days by " & SWITCH(SELECTEDVALUE('CC Dup Parameter'[CC Dup Parameter Order]), 0, "City", 1, "Category")`

