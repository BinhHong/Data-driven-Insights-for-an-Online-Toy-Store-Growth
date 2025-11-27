# Description

Introdudction


## Order items and Orders
- Orders table: for each order_id there are information on product_id of the primary product (when there is a bundle purchase), the  number of purchased items, total price and total cost as well as the website session and user_id. So we lack information on which items are purchased, especially when it's a bunble purchase. These are provided in the table Order Items, where for each order_item_id we can find which order_id is associated and exact which products are ordered.
