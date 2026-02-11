# Introduction
This project presents a set of Power BI dashboards designed to analyze the business performance of an online toy store. The goal is to provide clear, insightful views of growth trends and to derive actionable insights that can help improve overall business performance.
# ETL Process
Data extraction, transformation, and loading (ETL) are performed in Power Query. The main steps include:
- After extraction, the data is carefully cleaned and analyzed, followed by multiple transformations.
- `Orders` and `Order Items` tables may appear similar but serve different purposes: `Orders` table contains order-level information such as total price and total cost while `Order Items` table contains item-level details, listing every product included in each order.
- `Orders` table: add a column Bundle Purchase which takes values "Y" if items_purchased >=2 and "N" otherwise
<!-- This is a GitHub README 

- in Orders table, for each order_id there are information on product_id of the primary product (when there is a bundle purchase), the total number of purchased items, total price and total cost as well as the website session and user_id. So we lack information on which items are purchased, especially when it's a bundle purchase. These are provided in the table Order Items, where for each order_item_id we can find which order_id is associated and exact which product are ordered, each product is listed corresponding to an order_item_id.

-->
- `Order Item Refunds` table: for each order_item_refund_id there is an order_item_id. Because each order_item_id is associated with only one product, we might see the case that an order_id appears many times in an order_item_id. There are total 1731 rows in this table and only 1723 distinct order_id's, which means that in some bundle orders many or all products are returned.
- `Website Sessions` and `Websites Pageviews` tables: each website session contains some website page views, which tells which Page are viewed by users
- `Products` table: add columns `Price` and `Cost` for each product by **Group By** and **Merge Queries**
- using M Code to create a table `Calendar` starting from 2012-03-19 to 2015-03-31 and add columns `Year`, `Start of Month`,`Start of Quarter`, `Start of Week` and `Day Name`

# Data Modelling
- first of all, the PK's are defined for each table
- the field website_session_id in table `Orders` has distinct values and not null. So it's safe to set up a one-to-one relationship with the table `Website Sessions` and set Cross-filter direction as Both
- 3 dimension tables are `Calendar`, `Products` and `Website Sessions` and 3 fact tables are `Order Items`, `Order Item Refunds` and `Orders`
- the tables `Data Dictionary` and `Measure Table` are isolated
- the FK order_id, created_at and product_id in fact table `Order Items` as well as order_id and created_at in the table `Order Item Refunds` are hidden in Report View
- in Table View, all date fields are updated using Short Date format. Furthermore, the fields price_usd and cogs_usd are updated to currency format
- adding DAX calculated columns and measures:
    - Calculated columns:`Weekend` for the table `Calendar`, `Years Since Launch` for the table `Products` using **Time Intelligence**, `Price Tier` in the table `Products` and `Traffic Source` for the table `Website Sessions`
    - Measures: `Quantity Sold` and `Quantity Returned`, `Total Orders`, `Total Separate Orders` and `Total Returns`, `Return Rate`, `Weekend Orders` and `% Weekend Orders`, `Total Revenue`, `Total Cost`, `Total Profit` and `Profit Margin`, `YTD Revenue` and `60-Day Revenue`, `Last Month Revenue`, `Last Month Profit`, `Last Month Orders`, `Last Month Returns`, `Revenue Target`, `Orders Target`, `Profit Target`, `Total Sessions` and `Total Pageviews`
- Create a table `Measure Table` in Power Query Editor and load to Front-End
- finally a snowflake schema is used

![](Images/Data_modelling.png?raw=true)



#  Visualization
- 3 reports are created: `Executive Dashboard`, `Product Details` and `Website Details`
- edit interactions in each Page
- add some bookmarks
- add a button to clear all filters for Exec Page
- add navigation buttons to move betweeen pages conviniently

**Executive Dashboard**

![Executive Dashboard](Images/exec.png?raw=true)

**Product Details**

![Product Details](Images/product.png?raw=true)

**Website Details**

![Website Details](Images/website.png?raw=true)


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
* For each `order_id`, the `Orders` table includes information about the primary product (in bundle purchases), total number of items purchased, total price, total cost, website session, and `user_id`. However, detailed product-level information—especially for bundle purchases—is not available at this level.
* Detailed product information is provided by the `Order Items` table, where each `order_item_id` is linked to an `order_id` and a specific product.
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
* The `Data Dictionary` and `Measure Table` are kept isolated from the main schema.
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
