# Description

Introdudction
# ETL
- in each table Orders, Order Items, Order Item Refunds, Website Pageviews and Website Sessions: add calculated columns Day Name, Year, Start of Month, Start of Week and change data type to Date for Start of Month and Start of Week
-- add a column Bundle Purchase which takes values "Y" if items_purchased >=2 and "N" otherwise

## Order items and Orders
- Orders table: for each order_id there are information on product_id of the primary product (when there is a bundle purchase), the total number of purchased items, total price and total cost as well as the website session and user_id. So we lack information on which items are purchased, especially when it's a bunble purchase. These are provided in the table Order Items, where for each order_item_id we can find which order_id is associated and exact which product are ordered, each product is listed corresponding to an order_item_id.

## Order Item Refunds table:
- for each order_item_refund_id we have an order_item_id. Because each order_item_id is associated with only one product, we might see the case that an order_id appears many times in an order_item_id. There are total 1731 rows in this table and only 1723 distinct order_id's, which means that in some bundle orders many or all products are returned.

## Website Sessions and Websites Pageviews
- each website session contains some website page views, which tells which Page are viewed by users

# Data Modelling
- the field website_session_id in table Orders has distinct values and not null. So it's safe to set up a one-to-one relationship with the table Website Sessions and set Cross-filter direction as Both
- 2 dimension tables are Products and Orders and 2 fact tables are Order Items and Order Item Refunds
- the table Data Dictionary is isolated
