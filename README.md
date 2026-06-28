# Data Profiling and Tableau Preparation Guide

## Overview
This document outlines the data structure, field classifications, and assumptions for the `dashboard_sales_data.csv` dataset prior to loading it into Tableau. 

## Field Classifications & Tableau Data Types

###  Date Fields
*Tableau will automatically recognize these as Date types based on the `YYYY-MM-DD` format.*
* `order_date`: Date (Discrete/Continuous)
* `ship_date`: Date (Discrete/Continuous)

###  Geographic Fields
*These fields require assigning Geographic Roles in Tableau.*
* `state`: String (Assign Geographic Role: State/Province)
* `city`: String (Assign Geographic Role: City)
* `region`: String (Typically a standard Dimension, unless custom geographic territories are built in Tableau).

### Categorical Fields (Dimensions)
*Text-based fields used for slicing and grouping data.*
* `order_id`: String
* `customer_id`: String
* `customer_segment`: String (Values: Consumer, Corporate, Home Office)
* `category`: String
* `sub_category`: String
* `product_name`: String
* `ship_mode`: String
* `campaign_channel`: String

### Numerical Measures
*Continuous variables that Tableau will automatically aggregate (e.g., SUM, AVG).*
* `sales`: Float (Format as Currency)
* `quantity`: Integer
* `discount`: Float (Format as Percentage)
* `profit`: Float (Format as Currency)
* `delivery_days`: Integer
* `customer_rating`: Float (Range: 1.0 - 5.0. Best visualized as an Average rather than Sum).

###  Binary / Flag Fields
* `return_flag`: Integer (0 or 1)

---

## Assumptions & Tableau Setup Notes

1.  **Geographic Mapping Context:** The states (e.g., Punjab, Rajasthan, Goa) and cities (e.g., Ludhiana, Jaipur) are located in India. To generate accurate filled maps or point maps, Tableau's map location settings must be configured to default to **Country: India**.
2.  **Binary Field Conversion:** Because `return_flag` is numeric (0/1), Tableau will initially load it as a **Measure** (continuous). To use this correctly for filtering or color-coding returned vs. non-returned orders, it must be dragged into the **Dimensions** pane (converted to discrete) upon loading.
3.  **Delivery Days:** `delivery_days` is calculated as the difference between `order_date` and `ship_date`. While it is a number, it may occasionally be useful to convert it to a Dimension to analyze the frequency distribution of specific shipping durations.
4.  **Discount Formatting:** `discount` is provided as a decimal (e.g., 0.25). In Tableau, the default number format for this measure should be updated to Percentage so it displays as 25% on tooltips and labels.
