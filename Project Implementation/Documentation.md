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

---

## Phase 4: Dashboard Design

### Color Pallet:

Color Hex Codes: `#0077B6 (Dark), #00B4D8 (Medium) & #90E0EF (Light)`.

Color Standard Assignment:
1. City: Ahmedabad: Dark, Surat: Medium & Vadodara: Light.
2. Product Category: Beverages: Dark, Dairy: Medium & Food: Light.
3. Delay: 3 Days: Dark, 2 Days: Medium & 1 Day: Light.

### I. Glance at KPIs:

- Created Visual Blocks for `OT %`, `IF %` & `OTIF %` KPIs comprising of 3 components: 
a. Gauge Chart for the Metric value with target set to the calculated metric target. 
b. Orders Card shows the total orders for the metric.
c. Target & Variance Card for the the metric. Variance has been conditionally formatted to Red color for negative values i.e Actual < Target value.
- Added Info Button to all the KPI Visual Blocks providing a brief explanation for the same.
- Added Monthly (Mmm YY format) slicer to filter on specific time frame.

### II. Glance at Cities:

- Created Product Category Orders by City - Stacked Bar Chart displaying the split of Order lines per category across the 3 cities.
- Created a dynamic KPI vs Target by City - Clustered Bar Chart displaying the OT %, IF % & OTIF % KPI values vs targets comparison across the 3 cities. Configured a field parameter:
`Base KPIs Parameter = {("OT %", NAMEOF('Key Measures'[OT %]), 0), ("IF %", NAMEOF('Key Measures'[IF %]), 1), ("OTIF %", NAMEOF('Key Measures'[OTIF %]), 2)}`
This parameter was used as a Slicer and as the dynamic X Axis input to change the KPI plotted as per the selection made on the Parameter Slicer.
- `KPIs vs City Title` Measure was used as a card to display a dynamic title that changes depending on the KPI selected.

### III. Gaze at KPI Trends:

- Configured a field parameter:
`All KPIs Parameter = {("OT %", NAMEOF('Key Measures'[OT %]), 0), ("IF %", NAMEOF('Key Measures'[IF %]), 1), ("OTIF %", NAMEOF('Key Measures'[OTIF %]), 2), ("LiFR %", NAMEOF('Key Measures'[LiFR %]), 3), ("VoFR %", NAMEOF('Key Measures'[VoFR %]), 4)}`
This parameter was added as a Slicer and was used as a dynamic Y Axis input for the Line Chart displaying KPI (OT %, IF %, OTIF %, LiFR % & VoFR %) Trend over Time. KPI plotted is as per the selection made on the Parameter Slicer. 
Target Measures for the corresponding KPIs were added as a Constant Y Axis Reference line. `KPIs Performance Title` Measure was used as a card to display a dynamic title that changes depending on the KPI plotted.
- Configured a field parameter:
`CC Parameter = {("City", NAMEOF('dim_customer'[city]), 0), ("Category", NAMEOF('dim_product'[category]), 1)}`
This parameter was added as a Slicer and was used as a dynamic Legend input for the Ribbon Chart displaying KPI (plotted as per the selection made on the `All KPIs Parameter` Slicer above) Breakdown Trend. 
Breakdown criteria (City or Category) is based on the selection made on the `CC Parameter` slicer.
Trend can be drilled down from monthly to weekly to daily levels.
`KPIs Breakdown Title` Measure was used as a card to display a dynamic title that changes depending on the KPI plotted and Breakdown criteria selected.

### IV. Gaze at Customers:

- Created a Matrix visual displaying the KPIs (OT %, IF %, OTIF %, LiFR % & VoFR %) Breakdown across customers. The visual was conditionally formatted as follows:
For `OT % / IF % / OTIF %` Metrics: Color becomes darker with favorable lower target variance. 
For `LiFR % / VoFR %` Metrics: Color becomes darker with favorable higher metric value.
- Added City Slicer to enable filtering on specific cities.
- Created a OT % vs IF % Segmentation - Scatter plot for all customers. Two Constant X and Y Axis Reference Lines were added at 50% value to divide the plot in 4 quadrants. Q1 indicates customers with good values across both metrics. Q2 & Q4 indicate customers with issues in one metric i.e either lower InFull % or On Time %. Q3 indicates the customers with bad values across both metrics. Zoom Sliders were added to enable further data drilling.

### V. Gaze at Products:

- Created Visual Blocks for LiFR % & VoFR % KPIs comprising of 2 components: 
a. Gauge Chart for the Metric value. No target was set as target data not available for these metrics. Info Button was added to provide a brief description for the metric.
b. Cards showing: Total Order Lines & In Full Order Lines for LiFR % metric and Total Ordered Qty and Total Delivered Qty for VoFR % metric.
- Created a Matrix visual displaying LiFR % and VoFR % Breakdown by Product Category. Data can be drilled down to Product Type and Product Name levels. Additionally, Sparklines were configured to display the trend over month for both metrics with markers on lowest and highest values.
- Added Monthly (Mmm YY format) slicer to filter on specific time frame.
- Created 4 Line & Clustered Column Charts to visualize the LiFR % and VoFR % Trend across City and Product Category along with Total Order Lines (for LiFR %) and Total Ordered Qty (for VoFR %). Four custom blank buttons were created for LiFR % and VoFR % Metrics to act as a toggle to switch between the 2 visual groups using bookmarks: `LiFR % Chart Shown` and `VoFR % Chart Shown`. These bookmarks enable displaying 2 visuals for each metric selected using toggle buttons. Charts can be drilled down at City to Customer and Product Category to Product Type levels.

### VI. Gaze at Customer Lead Times:

- Created a Visual Block for ADDD KPI comprising of 3 components: 
a. KPI Card for the Metric value. Info Button was added to provide a brief description for the metric. 
b. Card showing the count of Total Orders that were delayed at any limit.
c. Individual Cards showing the count of Orders delayed by 1, 2 and 3 days.
- Configured a field parameter:
`CC Dup Parameter = {("City", NAMEOF('dim_customer'[city]), 0), ("Category", NAMEOF('dim_product'[category]), 1)}`
This parameter was added as a Slicer and was used as a dynamic Legend input for the Stacked Bar Chart displaying Total Order Lines by Delivery Delay Days Breakdown. 
Existing `CC Parameter` was not reused to avoid unintentional filtering due to same parameter being used for 2 slicers at different places.
Breakdown criteria (City or Category) is based on the selection made on the `CC Dup Parameter` slicer.
`CC Delivery Delay Title` Measure was used as a card to display a dynamic title that changes depending on the Breakdown criteria selected.
- Created a Line & Stacked Column chart visualizing the Delivery Delay Days Breakdown and ADDD Metric Trend over customers. Columns have been color coded with No Delay and Early Delivery Order Cases as Neutral colors and Delay Orders as Standard Blue color palette (increasing darkness with higher delay).

---
