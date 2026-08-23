E-commerce Revenue Intelligence Dashboard


# Business Problem

An online toy retailer is selling products directly through its website. Revenue is generated through online product sales. The dataset captures the customer journey from website visit to refund.
This project analyzes the complete customer journey, from website acquisition through product refunds, to identify opportunities for improving business performance. The objective of this project is to identify the key drivers of traffic, conversion, revenue, and refunds across the customer journey and present actionable insights through an interactive Power BI dashboard.

Key business questions include:

- What drove revenue growth over time — higher traffic, stronger conversion, or increasing order value?
- How have orders, sessions, conversion rate, and Average Order Value evolved over time?
- Which traffic sources contribute the most acquisition volume, and which generate the strongest conversion efficiency and revenue per session?
- How does conversion performance differ between desktop and mobile, and does the device gap persist across traffic sources and comparable landing pages?
- How do new and repeat sessions differ in conversion and revenue efficiency, and did changes in repeat-session behavior contribute to overall conversion improvement?
- Which products drive revenue and gross profit, and how has their relative contribution changed as the portfolio expanded?
- How do product margin and refund risk differ across the portfolio?
- How important are bundle purchases to order value and revenue, and what drove the increase in AOV?
- Which landing pages attract the most traffic and conversions, and to what extent can differences in performance be explained by device composition and operating period?
- How have refund losses changed as the business has grown?
- Are refund losses and refund rate concentrated in specific products?
- Were major refund spikes caused by broad deterioration or isolated product-specific anomalies?

For detailed business context, see: docs/01_Business_Understanding.md

# ETL Process

Data extraction, transformation, and loading (ETL) are performed in **Power Query**. The main steps include:

* After extraction, the data is carefully cleaned and analyzed, followed by multiple transformations.
* The `Orders` and `Order Items` tables may appear similar but serve different purposes:

  * `Orders` contains order-level information such as total price and total cost.
  * `Order Items` contains item-level details, listing every product included in each order.
* In the `Orders` table, a calculated column **Bundle Purchase** is added:

  * Value **"Y"** if `items_purchased >= 2`
  * Value **"N"** otherwise
* The `Order Item Refunds` table links each `order_item_refund_id` to an `order_item_id`. Since each order item corresponds to a single product, a single `order_id` may appear multiple times. The table contains 1,731 rows but only 1,723 distinct `order_id` values, indicating that some bundle orders involve multiple returned products.
* The `Website Sessions` and `Website Pageviews` tables capture user behavior, where each website session consists of multiple page views.
* In the `Products` table, `Price` and `Cost` columns are created using **Group By** and **Merge Queries** operations.
* A `Calendar` table is generated using **M code**, covering the period from **2012-03-19** to **2015-04-01**, with additional columns:

  * `Year`
  * `Start of Month`
  * `Start of Quarter`
  * `Start of Week`
  * `Day Name`

---

# Data Modeling

The data model is designed following dimensional modeling best practices:

* Primary keys (PKs) are defined for each table.
* Dimension tables:

  * `Calendar`
  * `Products`
* Fact tables:

  * `Orders`
  * `Order Items`
  * `Order Item Refunds`
  * `Website Pageviews`
  * `Website Sessions`
* The `Data Dictionary` and `Measure Table` tables are kept isolated from the main schema.
* Foreign key fields (`order_id`, `created_at`, `product_id`) in the `Order Items` table and (`order_id`, `created_at`) in the `Order Item Refunds` table are hidden in Report View.
* In Table View, all date fields use the **Short Date** format, while `price_usd` and `cogs_usd` fields are formatted as currency.

### DAX Calculations

* **Calculated Columns**:

  * `Weekend` (Calendar)
  * `Years Since Launch` (Products, using Time Intelligence)
  * `Price Tier` (Products)
  * `Traffic Source` (Website Sessions)

* **Measures**:

  * Sales & returns: `Quantity Sold`, `Quantity Returned`, `Total Orders`, `Total Separate Orders`, `Total Returns`, `Return Rate`
  * Time-based metrics: `Weekend Orders`, `% Weekend Orders`, `YTD Revenue`, `60-Day Revenue`, `Last Month Revenue`, `Last Month Profit`, `Last Month Orders`, `Last Month Returns`
  * Financial KPIs: `Total Revenue`, `Total Cost`, `Total Profit`, `Profit Margin`
  * Targets: `Revenue Target`, `Orders Target`, `Profit Target`
  * Web analytics: `Total Sessions`, `Total Pageviews`, `Conversion Rate`

* A dedicated `Measure Table` is created in Power Query and loaded into the front-end to organize all measures.

* The final model follows a **galaxy schema** design.

![Data Model](Images/modelling.png?raw=true)

---

# Visualization

Three interactive Power BI reports are developed:

* **Executive Dashboard**
* **Product Details**
* **Website Details**

Key visualization features include:

* Customized visual interactions on each report page to control filtering behavior
* Use of bookmarks to enhance navigation and support storytelling
* A dedicated “Clear Filters” button on the Executive Dashboard for improved usability
* Navigation buttons for seamless movement between report pages

### Executive Dashboard

![Executive Dashboard](Images/exec.png?raw=true)

### Product Details

![Product Details](Images/product.png?raw=true)

### Website Details

![Website Details](Images/website.png?raw=true)


# Key Insights and Recommendations
A dedicated **Key Insights and Recommendations** page is included as the final section of the report. This page consolidates the most important analytical findings from the Executive, Product, and Website dashboards and translates them into actionable business recommendations.

The objective of this page is to move beyond data visualization and provide strategic direction based on performance trends, product profitability, return behavior, and conversion rate analysis.

![](Images/key_insight.png?raw=true)


# Next:
- create date table (done)
- correct Product table, no need transformation (done)
- correct readme with business questions and etl process
- add order_date in Website sessions, connect Calendar to Sessions and set inactive (done)
- add a date in Website sessions for Pageview, connect Calendar to Sessions and set inactive (done)
- creating metrics in order
 base: order and product revenue and cost, orders, sessions, converted sessions, conversion rate, refund amount, refunded items,.... (done)
- check redundant metrics (done)
- time intelligence metrics (done)
- validation: order revenue = product revenue?
- check until 8. xong Bundle Orders (done)
- KPI docu till 04.Product metrics

# Technical Steps:
- create a report-friendly DAX category such as "New" / "Repeat" and hide the raw 0/1 flag.

# 9. Business Question to KPI Mapping

| Business Question | Main KPIs |
|---|---|
| How has revenue changed over time? | Revenue by Order Date, Revenue MoM %, YTD Revenue |
| What are the trends in orders, sessions, and conversion rate over time? | Orders by Order Date, Sessions, Conversion Rate, Orders MoM %, Sessions MoM %, Conversion Rate MoM pp |
| What is the Average Order Value, and how has it changed over time? | Average Order Value, Previous Month AOV, AOV MoM % |
| Which traffic sources generate the most sessions, orders, and revenue? | Sessions, Converted Sessions / Orders, Order Revenue |
| Which traffic sources have the highest conversion rates? | Conversion Rate |
| How do new and repeat visitors differ in conversion performance? | Sessions, Converted Sessions, Conversion Rate, Revenue per Session |
| Which products generate the highest revenue and gross profit? | Product Revenue, Product Gross Profit, Product Profit Margin, Units Sold by Product |
| Which products have the highest refund rates? | Product Refund Rate, Refunded Items |
| How common are bundle purchases, and what impact do they have on revenue? | Bundle Orders, Bundle Order Share, Revenue by Order Date, Average Order Value |
| How efficiently do website sessions convert into completed orders? | Sessions, Converted Sessions, Conversion Rate, Revenue per Session |
| Which landing pages attract the most traffic and conversions? | Sessions, Converted Sessions, Conversion Rate |
| How much revenue is lost through refunds over time? | Refund Amount, Previous Month Refund Amount, Refund Amount MoM % |
| Are refunds concentrated in specific products? | Refund Amount, Refunded Items, Product Refund Rate |
| Which devices generate the best conversion performance? | Sessions, Converted Sessions, Conversion Rate, Revenue per Session |


# 11. Metric Design Principles

The following principles are applied throughout the semantic model:

1. **Measures are preferred over implicit aggregations.**
2. **Business metrics are centralized in a dedicated measure table.**
3. **Different analytical grains are kept explicit.**
   - Orders are used for order-level economics.
   - Order Items are used for product-level economics.
   - Website Sessions are used for acquisition and conversion.
   - Refunds are used for return activity.
4. **Revenue is not treated as a single universal measure.**
   - `Order Revenue` supports session-based analysis.
   - `Revenue by Order Date` supports sales trends.
   - `Product Revenue` supports product analysis.
5. **Session Date and Order Date are intentionally separated.**
6. **Conversion is defined at session grain.**
7. **Product refund rate is not interpreted as a monthly cohort metric without additional cohort logic.**
8. **Dimensions provide segmentation; duplicate segment-specific measures are avoided.**
9. **Numeric measures return numeric values or BLANK, not text fallback values.**
10. **Time-intelligence measures are created only when they support a defined business question.**



# 12. KPI Framework Summary

The metric framework is organized into six analytical domains:

- **Sales** — revenue, orders, profitability, AOV, and bundle behavior
- **Product** — units, product revenue, product profitability, and product mix
- **Website & Conversion** — sessions, converted sessions, conversion rate, and revenue efficiency
- **Customer & Visitor** — unique visitors and repeat engagement
- **Returns** — refund value, refunded items/orders, and product refund rate
- **Time Intelligence** — month-over-month and year-to-date performance comparisons

Together, these metrics provide the analytical foundation required to answer the project's selected business questions while maintaining clear definitions, consistent date logic, and appropriate fact-table grain.