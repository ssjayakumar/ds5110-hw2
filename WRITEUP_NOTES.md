STEP 0: DESCRIPTIVE ANALYSIS

The revenue file (5 rows × 13 columns) is not tidy. "Quarter" is a variable, but its values are column headers, and each row holds 12 observations instead of one. The orders file (20 × 5) is tidy, with one row per order. The shipments file (35 × 2) is also tidy, with one row per shipment, so an order_id can appear more than once.

STEP 1: RESHAPING THE REVENUE TABLE

I used melt to make one row per (company, quarter), giving 60 rows (5 companies × 12 quarters). The raw file broke the rule that each variable must be a column: quarter was spread across 12 column headers.

STEP 2: CHECKING THE KEYS

order_id is unique in orders but repeats in shipments (up to 3 times per order). So the join is one-to-many and will repeat each order once per shipment: 35 rows instead of 20.

STEP 3: JOINING ORDERS AND SHIPMENT

The direct join gave 35 rows, as predicted, and four customers got a different mean amount. C107, for example, was 106.00 on the joined table but 96.50 on orders alone. The orders-only number is correct, because the join counts an order's amount once per shipment, so orders with more shipments weigh more. I fixed this by counting shipments per order first, keeping one row per order.

STEP 4: ANALYSIS READY TABLE

The final table has 20 rows, one per order, with columns order_id, customer_id, company, quarter, order_amount, n_shipments and company_quarter_revenue. Revenue was joined on company and quarter (many-to-one). I checked that there are still 20 rows, order_id is unique and no revenue is missing.

STEP 5: VISUALIZATION

The new chart plots each order against its company's revenue that quarter, sized by number of shipments. It needs all three files together. With the raw files I would have had to match each order to the wide revenue grid by hand; after tidying and joining, everything is in one row, so it takes one plotting call.