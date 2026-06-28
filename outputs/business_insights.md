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
