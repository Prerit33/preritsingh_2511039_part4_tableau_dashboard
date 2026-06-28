# Task 6
# Dashboard Design & Chart Selection Justification

## 1. Chart Selection Rationale

* **Sales Trend View (Line Chart):** * *Justification:* Line charts are the industry standard for time-series data. They intuitively display the flow of time from left to right, making it easy for executives to spot seasonality, growth trends, or sudden dips in sales volume.
* **Regional Performance (Bar Chart with Color Encoding):**
    * *Justification:* Horizontal bar charts are ideal for geographic or categorical data with long text labels (like state names), ensuring high readability without awkward text rotation. By using bar *length* for Sales and bar *color* for Profit Margin, we encode two vital metrics into a single, uncluttered view.
* **Category Profitability (Sorted Bar Chart):**
    * *Justification:* Bar charts allow for precise length comparisons. By sorting descending by profit, we immediately draw the user's eye to the most and least profitable sub-categories, eliminating the need for the user to search for insights.
* **Customer Segment (Bar Chart with AOV Tooltips):**
    * *Justification:* A simple bar chart prevents cognitive overload when comparing just three or four segments. Moving the Average Order Value (AOV) to the tooltip keeps the primary view clean while preserving deeper analytical capabilities on demand.
* **Shipping Performance (Stacked Bar Chart):**
    * *Justification:* Stacked bars perfectly illustrate part-to-whole relationships. They allow us to see the total volume of orders per shipping mode while simultaneously revealing the proportion of fast, standard, and delayed shipments within those modes.
* **Discount vs. Profit (Scatter Plot):**
    * *Justification:* Scatter plots are the only effective way to visualize the correlation between two continuous measures (Discount and Profit). It quickly reveals whether deeper discounts drive volume or simply erode margins.
* **Return Analysis (Bar Chart):**
    * *Justification:* A standard bar chart explicitly formatted with percentage axes allows for rapid comparison of return rates across categories, highlighting operational bottlenecks.

## 2. Design Principles Applied

* **Clear Hierarchy:** The dashboard is structured using an "F-pattern" reading layout. High-level summary metrics (KPIs) are positioned at the very top. The user's eye then moves down to the broadest context (Sales Trend), and finally down to granular, actionable cuts of data (Regions, Categories).
* **Minimal Clutter & Proper Labels:** Data ink is maximized by removing redundant axes, unnecessary background gridlines, and heavy borders. All axes are clearly labeled with appropriate units (e.g., Currency formatting for Sales, Percentage for Margins/Returns) so no mental math is required.
* **Consistent Color Usage:** Color is used purposely, not decoratively. A consistent semantic palette is applied (e.g., if Blue represents profitability and Orange represents loss/delays in one chart, that logic persists globally). 
* **Appropriate Sorting:** Alphabetical sorting is avoided in favor of value-based sorting (highest to lowest). This immediately ranks performance and answers the business question of "who are our top/bottom performers?"
* **Avoidance of Misleading Scales:** Bar chart axes are pinned to zero to ensure length comparisons are visually accurate. Scatter plot axes are allowed to float to highlight data clustering, but trendlines clearly indicate the mathematical correlation to prevent misinterpretation.
* **Useful Filters & Actions:** Filters (Region, Category, Date) are configured as dropdowns to save space and are wired to update *all* charts simultaneously. Furthermore, enabling "Use as Filter" on the charts allows the dashboard to act as a fully interactive, drill-down application rather than a static report.

## 3. Focus on Business Interpretation
Every element on this dashboard is designed to answer specific business questions: *Are we growing? Where are we losing money? Do discounts actually help? Are shipping delays causing returns?* By combining clean visualizations with strategic interactivity, the dashboard empowers executives to move from data observation to business action in seconds.

# Task 10

# Chart Selection & Design Justification

### 1. Sales Trend View
1. **Question Answered:** How are sales trending over time, and are there seasonal patterns or recurring dips?
2. **Chart Type Appropriateness:** A Line Chart is the industry standard for time-series data because it intuitively connects data points, visually representing the continuous flow of time.
3. **Field Usage:** `order_date` (set to continuous Month) is used for the X-axis, and `SUM(sales)` is used for the Y-axis size. `category` can be applied as a filter.
4. **Design Principle Applied:** High Data-Ink Ratio. Removed background gridlines to ensure the trendline stands out without visual clutter.
5. **Mistake Avoided:** Avoided using a bar chart for time-series data, which can break the visual continuity and make it harder to spot overall growth or decline trajectories.

### 2. Regional Performance View
1. **Question Answered:** Which regions generate the most sales, and are those high-revenue regions actually profitable?
2. **Chart Type Appropriateness:** A Horizontal Bar Chart is ideal for categorical data with long text labels (like state names), allowing viewers to read the labels without tilting their heads.
3. **Field Usage:** `SUM(sales)` dictates bar length (size), `state/region` serves as the row label, and the `Profit Margin` calculated field is applied to Color.
4. **Design Principle Applied:** Dual Encoding. By using length for revenue and color for profitability, the chart packs two crucial metrics into one clean view.
5. **Mistake Avoided:** Avoided alphabetical sorting. Sorted the bars descending by sales volume to immediately answer "who is our top performer?" without forcing the user to search.

### 3. Category Profitability View
1. **Question Answered:** Which specific product sub-categories are driving profits or causing losses?
2. **Chart Type Appropriateness:** A Bar Chart allows for highly accurate visual comparisons of length, making it easy to see marginal differences in profit between closely ranked products.
3. **Field Usage:** `category` and `sub_category` are used for hierarchical labels, and `SUM(profit)` dictates the bar size (length).
4. **Design Principle Applied:** Visual Hierarchy. Grouping sub-categories strictly under their parent categories to provide structural context to the data.
5. **Mistake Avoided:** Avoided using a pie chart or packed bubble chart, which are notoriously difficult for the human eye to parse when comparing more than 3-4 similar values.

### 4. Customer Segment View
1. **Question Answered:** How do different customer segments contribute to overall sales, and what is their purchasing behavior?
2. **Chart Type Appropriateness:** A standard Bar Chart provides a simple, unmistakable comparison between a small number of distinct categories.
3. **Field Usage:** `customer_segment` acts as the label, `SUM(sales)` defines the size, and `Average Order Value` is explicitly used in the Tooltip.
4. **Design Principle Applied:** Progressive Disclosure. Kept the main visual simple (just sales by segment) but embedded the secondary metric (AOV) in the tooltip for on-demand exploration.
5. **Mistake Avoided:** Avoided overcomplicating a simple metric with 3D effects or unnecessary color gradients that add no informational value.

### 5. Shipping Performance View
1. **Question Answered:** How do delivery delays vary across different shipping modes?
2. **Chart Type Appropriateness:** A Stacked Bar Chart is perfect for showing part-to-whole relationships, illustrating both total order volume and the internal composition of delivery speeds.
3. **Field Usage:** `ship_mode` is used for the axis label, `COUNTD(order_id)` determines the bar size, and `Shipping Delay Bucket` is applied to Color.
4. **Design Principle Applied:** Semantic Color Usage. Used a traffic-light color scheme (e.g., green for fast, yellow for standard, red for delayed) to make interpretation intuitive.
5. **Mistake Avoided:** Avoided side-by-side (grouped) bar charts, which would make it difficult to estimate the *total* volume of orders per shipping mode at a glance.

### 6. Discount vs. Profit View
1. **Question Answered:** Does offering deeper discounts correlate with lower profitability, and where is the breaking point?
2. **Chart Type Appropriateness:** A Scatter Plot is the only effective way to visualize the correlation (or lack thereof) between two continuous numerical variables.
3. **Field Usage:** `AVG(discount)` is used for the X-axis, `SUM(profit)` for the Y-axis, and `sub_category` is placed on Detail to generate individual data points.
4. **Design Principle Applied:** Contextualization. Added a trendline to mathematically clarify the relationship, reducing user guesswork.
5. **Mistake Avoided:** Avoided using a line chart, which incorrectly implies a sequential relationship between data points that are actually independent events.

### 7. Return Analysis View
1. **Question Answered:** Which product categories suffer from the highest rate of customer returns?
2. **Chart Type Appropriateness:** A Bar Chart ensures that percentages can be compared linearly and accurately.
3. **Field Usage:** `category` is used as the label, and the `Return Rate` calculated field dictates the size (bar length).
4. **Design Principle Applied:** Standardized Formatting. Explicitly formatted the axis as percentages rather than decimals to align with standard business terminology.
5. **Mistake Avoided:** Avoided graphing the absolute *count* of returns, which is a misleading scale. High-volume categories will naturally have more returns, so using a normalized *rate* prevents misinterpretation.
