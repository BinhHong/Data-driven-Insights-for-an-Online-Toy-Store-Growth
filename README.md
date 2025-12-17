# Introduction

# ETL
- add a column Bundle Purchase which takes values "Y" if items_purchased >=2 and "N" otherwise
- in Orders table, for each order_id there are information on product_id of the primary product (when there is a bundle purchase), the total number of purchased items, total price and total cost as well as the website session and user_id. So we lack information on which items are purchased, especially when it's a bunble purchase. These are provided in the table Order Items, where for each order_item_id we can find which order_id is associated and exact which product are ordered, each product is listed corresponding to an order_item_id.
- for each order_item_refund_id in the table Order Item Refunds we have an order_item_id. Because each order_item_id is associated with only one product, we might see the case that an order_id appears many times in an order_item_id. There are total 1731 rows in this table and only 1723 distinct order_id's, which means that in some bundle orders many or all products are returned.
- tables Website Sessions and Websites Pageviews: each website session contains some website page views, which tells which Page are viewed by users
- add columns `Price` and `Cost` for each product in `Products` table by **Group By** and **Merge Queries**
## Add a table Calendar
- using M Code to create a table `Calendar` starting from 2012-03-19 to 2015-03-31
- add columns `Year`, `Start of Month`,`Start of Quarter`, `Start of Week` and `Day Name`

# Data Modelling
- the field website_session_id in table Orders has distinct values and not null. So it's safe to set up a one-to-one relationship with the table Website Sessions and set Cross-filter direction as Both
- 3 dimension tables are `Calendar`, `Products` and `Website Sessions` and 3 fact tables are `Order Items`, `Order Item Refunds` and `Orders`
- the tables `Data Dictionary` and `Measure Table` are isolated
- the FK order_id, created_at and product_id in fact table Order Items as well as order_id and created_at in the table Order Item Refunds are hidden in Report View
- in Table View, all date fields are updated using Short Date format. Furthermore, the fields price_usd and cogs_usd are updated to currency format

#  Adding DAX calculated columns and measures
- Calculated columns:
    - add column `Weekend` for the table `Calendar`
    - add column `Years Since Launch` in the table `Products` using **Time Intelligence**
    - add column `Price Tier` in the table `Products`
    - add column `Traffic Source` in the table `Website Sessions`
- Measures:
    - `Quantity Sold` and `Quantity Returned`
    - `Total Orders` and `Total Returns`
    - `Return Rate`
    - `Weekend Orders` and `% Weekend Orders`
    - `Total Revenue`, `Total Cost`, `Total Profit` and `Profit Margin`
    - `YTD Revenue` and `60-Day Revenue`
    - `Last Month Revenue`, `Last Month Profit`, `Last Month Orders`, `Last Month Returns`
    - `Revenue Target`, `Orders Target`
    - `Total Sessions` and `Total Pageviews`
- Create a table `Measure Table` in Power Query Editor and load to Front-End



# Fragen
- Website Sessions?
- which sessions bring purchases? from which websites
- most viewed pages?