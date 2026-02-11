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
* The field `website_session_id` in the `Orders` table contains distinct, non-null values, allowing a one-to-one relationship with the `Website Sessions` table. The cross-filter direction is set to **Both**.
* Dimension tables:

  * `Calendar`
  * `Products`
  * `Website Sessions`
* Fact tables:

  * `Orders`
  * `Order Items`
  * `Order Item Refunds`
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
  * Web analytics: `Total Sessions`, `Total Pageviews`

* A dedicated `Measure Table` is created in Power Query and loaded into the front-end to organize all measures.

* The final model follows a **snowflake schema** design.

![Data Model](Images/Data_modelling.png?raw=true)

---

# Visualization

Three interactive Power BI reports are developed:

* **Executive Dashboard**
* **Product Details**
* **Website Details**

Key visualization features include:

* Edited visual interactions on each report page
* Use of bookmarks for enhanced navigation and storytelling
* A dedicated button on the Executive Dashboard to clear all filters
* Navigation buttons for seamless movement between report pages

### Executive Dashboard

![Executive Dashboard](Images/exec.png?raw=true)

### Product Details

![Product Details](Images/product.png?raw=true)

### Website Details

![Website Details](Images/website.png?raw=true)
