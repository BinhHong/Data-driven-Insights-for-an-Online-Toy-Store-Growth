# Introduction

This project presents a set of Power BI dashboards designed to analyze the business performance of an online toy store. The goal is to provide clear, insightful views of growth trends and to derive actionable insights that can help improve overall business performance.

---

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
* A `Calendar` table is generated using **M code**, covering the period from **2012-03-19** to **2015-03-31**, with additional columns:

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