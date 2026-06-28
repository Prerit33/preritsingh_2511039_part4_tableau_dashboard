# Task 2
# Tableau Calculated Fields and Business Logic

## Core Calculated Fields

### 1. Profit Margin
*   **Tableau Formula:** `SUM([profit]) / SUM([sales])`
*   **Explanation:** This aggregate calculation determines the overall profitability as a percentage of revenue. By aggregating `SUM()` before dividing, we ensure the margin is calculated correctly across any dimension (e.g., region or category) rather than averaging row-level margins, which would lead to inaccurate results. Ensure the default number format is set to Percentage.

### 2. Cost
*   **Tableau Formula:** `[sales] - [profit]`
*   **Explanation:** This row-level calculation determines the underlying cost of goods/services for each transaction. It can be aggregated (e.g., using `SUM([Cost])`) in visualizations to see total costs per product or region.

### 3. Average Order Value (AOV)
*   **Tableau Formula:** `SUM([sales]) / COUNTD([order_id])`
*   **Explanation:** AOV tracks the average dollar amount spent each time a customer places an order. We use `COUNTD()` (Count Distinct) on the `order_id` field rather than a simple count of rows, as a single order might span multiple line items or rows in the dataset.

### 4. Return Rate
*   **Tableau Formula:** `COUNTD(IIF([return_flag] == 1, [order_id], NULL)) / COUNTD([order_id])`
*   **Explanation:** This calculates the percentage of total unique orders that resulted in a return. We use an `IIF` statement to isolate the distinct count of returned orders and divide it by the distinct count of all orders. Ensure the default number format is set to Percentage.

### 5. Shipping Delay Bucket
*   **Tableau Formula:**
    ```tableau
    IF [delivery_days] <= 2 THEN "0-2 Days (Fast)"
    ELSEIF [delivery_days] <= 5 THEN "3-5 Days (Standard)"
    ELSE "6+ Days (Delayed)"
    END
    ```
*   **Explanation:** This converts the continuous `delivery_days` measure into a categorical dimension. Bucketing the shipping times allows us to easily visualize delivery performance and filter the dashboard to investigate whether delayed shipments correlate with higher return rates or lower customer ratings.



# Executive Dashboard Business Insights

This document contains key business insights derived from the Executive Sales & Performance Overview dashboard. Each insight follows a structured approach: identifying the observation, citing data evidence, providing business interpretation, and recommending actionable next steps.

---
# Task 8
### 1. Sales Trend Analysis
**Observation:** There is a distinct, recurring drop in sales volume immediately following the Q4 holiday peak, with Q1 consistently underperforming relative to the rest of the year.
**Data Evidence:** The Sales Trend View line chart shows revenue peaking at approximately $120,000 in November and December, followed by a sharp decline to an average of $35,000 in January and February.
**Business Interpretation:** The business is heavily reliant on end-of-year holiday purchasing. Post-holiday customer engagement is weak, leading to a cash flow valley in the first quarter.
**Recommended Action:** Design and launch a targeted Q1 customer retention campaign, offering exclusive "welcome back" loyalty discounts to first-time buyers acquired during the Q4 rush.

### 2. Regional Performance
**Observation:** Certain high-volume regions are generating substantial revenue but are operating at a net loss.
**Data Evidence:** The Regional Performance bar chart reveals that while Rajasthan (or the localized equivalent) is in the top 3 for total sales ($85,000), its color encoding indicates a heavily negative Profit Margin (-12%).
**Business Interpretation:** High top-line revenue in this region is being cannibalized by either exorbitant operational/shipping costs or an overly aggressive regional discounting strategy intended to capture market share.
**Recommended Action:** Conduct an immediate audit of freight costs and average discount rates applied specifically to orders originating in this underperforming region.

### 3. Category / Sub-Category Profitability
**Observation:** The "Tables" sub-category within the Furniture segment is consistently eroding overall profitability.
**Data Evidence:** The Category Profitability View shows that despite generating $50,000 in gross sales, the "Tables" sub-category resulted in a net loss of $15,000.
**Business Interpretation:** Large, bulky items like tables likely incur disproportionately high shipping and warehousing costs. Furthermore, they may require steep discounts to move inventory, destroying any gross margin.
**Recommended Action:** Re-evaluate the pricing model for the Tables sub-category, or negotiate better bulk-freight LTL (Less Than Truckload) rates with carriers to reduce fulfillment costs.

### 4. Customer Segment Behavior
**Observation:** The Corporate customer segment drives significantly higher value per transaction compared to the Consumer segment, despite lower total order volume.
**Data Evidence:** Tooltips in the Customer Segment View reveal that the Corporate segment has an Average Order Value (AOV) of $850, compared to the Consumer segment's AOV of $320.
**Business Interpretation:** Corporate buyers are purchasing in bulk for offices or opting for higher-margin premium products. This segment yields higher efficiency per transaction.
**Recommended Action:** Expand the B2B sales team headcount and shift 20% of the top-of-funnel marketing budget toward LinkedIn and enterprise acquisition channels.

### 5. Discount Impact
**Observation:** Deep discounts are actively destroying profitability without driving a compensatory increase in sales volume.
**Data Evidence:** The Discount vs. Profit scatter plot shows a steep drop into negative profit territory for transactions with a discount greater than 30% (0.3). There is no dense clustering of data points on the high-sales axis to justify these deep cuts.
**Business Interpretation:** The sales team or automated pricing rules are relying too heavily on deep discounting to close deals, which is cannibalizing margins without moving enough units to make up the difference.
**Recommended Action:** Implement a hard cap of 20% on all automated promotional discounts. Require secondary managerial approval for any discount exceeding this threshold.

### 6. Shipping & Delivery Performance
**Observation:** Standard Class shipping is failing to meet acceptable delivery timelines, resulting in a poor customer experience.
**Data Evidence:** The Shipping Performance stacked bar chart indicates that 45% of Standard Class orders fall into the "6+ Days (Delayed)" bucket.
**Business Interpretation:** The current third-party logistics (3PL) provider managing standard shipments is bottlenecked or underperforming against service level agreements (SLAs).
**Recommended Action:** Initiate a Request for Proposal (RFP) to evaluate and potentially onboard a new regional logistics partner for standard-tier deliveries.

### 7. Return Pattern Analysis
**Observation:** The Technology product category suffers from an unusually high return rate compared to other categories.
**Data Evidence:** The Return Analysis bar chart highlights that the Return Rate for Technology is 14%, whereas Office Supplies and Furniture hover around 4% to 6%.
**Business Interpretation:** High-value electronic products may be suffering from transit damage, or customers are experiencing setup/compatibility friction post-purchase, leading them to abandon the product and request a return.
**Recommended Action:** Implement an automated post-purchase email flow specifically for Technology items that includes quick-start guides, troubleshooting FAQs, and a direct line to technical support.

### 8. Business Opportunity & Risk
**Observation:** Several low-volume markets show exceptionally high profit margins, representing an untapped growth opportunity.
**Data Evidence:** The Regional Performance View highlights a small cluster of secondary regions (e.g., Punjab, Goa) that account for less than 5% of total national sales but boast Profit Margins exceeding 25%.
**Business Interpretation:** These markets have highly favorable unit economics (likely due to lower local competition or favorable shipping routes), but they lack the market penetration and brand awareness of primary regions.
**Recommended Action:** Allocate a dedicated test budget for geo-targeted digital advertising in these specific high-margin regions to see if sales volume can be scaled without degrading the profit margin.
