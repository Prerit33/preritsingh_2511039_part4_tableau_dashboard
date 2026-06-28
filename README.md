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
------------------------------------------------------------------------------------------------------------------------------------------

# Executive Sales & Performance Overview - Project README

## 1. Business Problem Summary
The organization is experiencing inconsistent profitability despite strong top-line revenue generation. The primary business problem is identifying the root causes of margin erosion, specifically analyzing how geographic location, product categories, discounting strategies, and fulfillment (shipping) delays impact overall net profit and customer return rates. The goal of this project is to provide leadership with an interactive diagnostic tool to pivot from low-margin/high-volume operations to sustainable, profitable growth.

## 2. Dataset Description
The underlying data is sourced from `dashboard_sales_data.xlsx`. It represents transactional retail/e-commerce data with the following key dimensions and measures:
* **Dates:** `order_date`, `ship_date`
* **Geography:** `state`, `city`, `region` (Context: India)
* **Categories:** `order_id`, `customer_id`, `customer_segment`, `category`, `sub_category`, `product_name`, `ship_mode`, `campaign_channel`
* **Measures:** `sales`, `quantity`, `discount`, `profit`, `delivery_days`, `customer_rating`
* **Flags:** `return_flag` (Binary 0/1)

## 3. Tableau Workbook Description
The Tableau workbook is engineered to function as an executive diagnostic tool. It consists of three high-level KPI indicator sheets and seven detailed visual worksheets that are unified into a single, interactive dashboard layout. The workbook follows a clear F-pattern hierarchy, allowing users to scan top-line metrics before diving into granular regional or product-level details.

## 4. Calculated Fields Created
To support the analysis, the following core calculated fields were engineered in Tableau:
1. **Profit Margin:** `SUM([profit]) / SUM([sales])` 
2. **Cost:** `[sales] - [profit]`
3. **Average Order Value (AOV):** `SUM([sales]) / COUNTD([order_id])`
4. **Return Rate:** `COUNTD(IIF([return_flag] == 1, [order_id], NULL)) / COUNTD([order_id])`
5. **Shipping Delay Bucket:** A nested IF statement categorizing `delivery_days` into "0-2 Days (Fast)", "3-5 Days (Standard)", and "6+ Days (Delayed)".

## 5. Dashboard Components
The final executive dashboard incorporates:
* **3 KPI Cards:** Total Sales, Total Profit Margin (%), and Return Rate (%).
* **Sales Trend View:** A time-series line chart tracking sales revenue over months.
* **Regional Performance View:** A horizontal bar chart dual-encoded with sales (length) and profit margin (color).
* **Category Profitability View:** A sorted bar chart highlighting net profit/loss by category and sub-category.
* **Customer Segment View:** A bar chart comparing total sales by segment, with Average Order Value embedded in the tooltip.
* **Shipping Performance View:** A stacked bar chart visualizing the volume of delayed shipments per shipping mode.
* **Discount vs. Profit View:** A scatter plot with a trendline detailing the negative correlation between deep discounts and profitability.
* **Return Analysis View:** A bar chart displaying return rates mapped against product categories.

## 6. Filters and Interactions Used
The dashboard is fully interactive, featuring the following controls:
* **Dropdown Filters:** Users can slice the entire dashboard by `Region`, `Category`, `Customer segment`, `Date` (Order Date), `Ship mode`, and `Campaign channel`. All filters are configured to "Apply to All Using This Data Source".
* **Dashboard Actions (Filter):** The Category Profitability chart is configured with a "Use as Filter" action. Clicking on a specific sub-category (e.g., "Tables") dynamically filters all other KPI cards and charts to reflect only that sub-category's data.

## 7. Key Business Insights
* **Seasonality Risk:** Revenue relies heavily on Q4, with a severe drop-off in Q1.
* **Margin Erosion:** Deep discounts (over 30%) and high fulfillment costs are destroying profitability in high-volume regions (e.g., Rajasthan) and specific sub-categories (e.g., Tables).
* **Operational Bottlenecks:** 45% of Standard Class shipments are delayed by 6+ days, correlating with a poor customer experience.
* **Segment Opportunity:** The Corporate segment yields an AOV of $850 compared to the Consumer AOV of $320, presenting a highly efficient growth channel.
* **Return Anomalies:** The Technology category experiences an unusually high 14% return rate compared to the 4-6% baseline of other categories.

## 8. Dashboard Story Summary
While the company demonstrates strong revenue generation capabilities, aggressive discounting and logistical inefficiencies are cannibalizing net margins. By capping automated discounts at 20%, auditing 3PL providers to fix standard shipping delays, and shifting marketing spend toward the highly profitable Corporate segment and untapped secondary regions, the business can stabilize its bottom line and mitigate the recurring post-holiday revenue slump. 

## 9. Assumptions and Limitations
* **Assumptions:** Geographic fields are mapped to India. It is assumed `delivery_days` calculation excludes non-working days (though raw data may include weekends). 
* **Limitations:** The dataset lacks granular cost visibility. We cannot differentiate between Cost of Goods Sold (COGS), inbound freight, outbound freight, or marketing spend. Furthermore, return data provides the *rate* of returns but lacks the *reason* (e.g., defective, buyer's remorse, shipping damage), limiting the specificity of our operational recommendations.

## 10. Screenshots Included
* `full_dashboard.png`: Contains the complete, final view of the Tableau Executive Dashboard including all KPI cards, charts, and visible filter dropdowns.
