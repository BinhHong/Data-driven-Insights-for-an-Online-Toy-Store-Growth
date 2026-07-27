

# Overview

## Data Dictionary
- there are 3 columns: Table, Field and Description
- the descriptions gives not only explanation to each Field in each Table but also indicate the PK and FK in each table

## Order Item Refunds
- Purpose: record the timestamp and amount of refunds to the order-item level
- PK: order_item_refund_id
- FK: order_item_id and order_id
- 1731 rows and 5 columns whose data types are all Text without Null values
- there are no missing values, empty strings, no duplicates
- Data types are then converted: 
order_item_refund_id, order_item_id, order_id: whole number
created_at: date
refund_amount_usd: decimal_number
- this table links each `order_item_refund_id` to an `order_item_id`. Since each order item corresponds to a single product, a single `order_id` may appear multiple times. The table contains 1,731 rows but only 1,723 distinct `order_id` values, indicating that some bundle orders involve multiple returned products.

## Order Items
- Purpose: provides details corresponding to each item that has been ordered to the grain of order-item. The information includes order-item-id which is PK, order_id and product_id which are FKs, created_at indicating the timestamp when the item was placed, is_primary_item being a flag determining if the item is primary in the order, price_usd and cogs_usd showing the price and cost of the product.
- 40025 rows. There are neither nulls, empty strings nor duplicates
- The following data types were changed:
order_item_id, order_id, product_id, is_primary_item: whole number
created_at: date
price_usd, cogs_usd: decimal_number

## Orders
- gives details on each order: each order_id (PK) has timestamp, website_session_id (FK) to which it belongs to, user_id (FK) showing which user placed the order, primary_product_id showing the id of the primary product in the order, items_purchased indicating the number of purchased products, price_usd and cogs_usd
- grain: order level
- 32313 rows. There are no nulls or empty strings, no duplicates
- data type for each column was changed:
order_id, website_session_id, user_id, primary_product_id, items_purchased: whole number
created_at: date
price_usd, cogs_usd: decimal number

## Products
- contains four products together with their names and creation dates.
- grain: product level
- There are no nulls or empty strings, no duplicates
- data types should be corrected:
product_id: whole number
created_at: date
product_name: text

## Website Pageviews
- for each website_pageview_id (PK) there are additional records on timestamp, the website_session_id (FK) it belongs to and the pageview_url
- grain: website pageview
- contains 1,188,124 rows with no null values, empty strings, or duplicate records.. 16 distinct pageview_url were recorded.
- data types should be corrected:
website_pageview_id, website_session_id: whole number
created_at: date

## Website Sessions
- each website_session_id (PK) was recorded together with information on timestamp, used_id (FK) referring the user who made this session, is_repeat_session indicating if the session is repeated, utm information, device_type and http_referer
- grain: website session, one row = one website session
- there are 472871 rows, no nulls or empty strings, no duplicates
- data types should be corrected:
website_session_id, user_id, is_repeat_session: whole number
created_at: date

# Cross-table Validation

The following business rules should be validated before production deployment:

- Order value equals the sum of its order item values.
- Items purchased equals the number of order items.
- Every order references a valid website session.
- Every order item references a valid product.
- Every refund references a valid order item.
- Refund amount does not exceed the original item price.

Based on the available profiling, no obvious referential integrity issues were identified.

# Modeling Implications

- Website Sessions is the starting point of the customer journey.
- Orders and Order Items represent different business grains and should remain separate fact tables.
- Products is the primary product dimension.
- Refunds should relate to Order Items rather than directly to Products.
- The Data Dictionary will be used to confirm relationships and key definitions during semantic modeling.

# Analytical Scope

Based on the available tables, the dataset supports analysis in the following business domains:

- Sales Performance
- Product Performance
- Marketing Performance
- Customer Journey
- Refund Analysis

The dataset does not support analyses requiring marketing costs, inventory, shipping, customer demographics, or geographic information.