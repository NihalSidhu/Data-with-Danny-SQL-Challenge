# Data Scrubbing
Prior to solving any of the case study questions, I needed to clean up the datasets. Just from initial glance at the provided tables, I noticed that some tables contained null values and other tables had inconsistent variable usage in their columns. Below I discuss the data cleaning I needed to do to the tables in order to make progress with the questions.

## Cleaning table customer_orders 
At initial glance of the customer_orders table I noticed that there were some issues with it. We can see a few issues, such as

- The 'exclusion's column has multiple values labeled as "null"
- The 'extras'  column has multiple values labeled as "null" or are actually null.

customer_orders table

![customer_orders](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/2b42cb09-925d-4d6d-a819-ea727da1a93b)

With the rogue data in these two columns, I needed to create a new cleaned version of the customer_orders. In order to do this, I replaced the dirty data with blanks in the new customer_orders_cleaned table. 

````sql
CREATE Table customer_orders_cleaned AS
SELECT 
order_id, 
customer_id,
pizza_id,
CASE 
	WHEN exclusions LIKE 'null' THEN ''
	ELSE exclusions
END AS exclusions,
CASE 
	WHEN extras LIKE 'null' OR extras IS NULL THEN ''
	ELSE extras
END AS extras,
order_time
FROM customer_orders
````
customer_orders_cleaned table

![customer_orders_cleaned](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/ad39e96c-feb9-4cf9-8579-017a80a86966)

## Table: runner_orders
Looking at runner_orders table we can see that there are values labeled "null" and NULL values throughout the table. Along with this, we can see that there are columns with different value types throughout. For example, the distance column has some values with "km" and some without. Similarly, the duration column has some with "minutes" or "mins".  I will create a new table labeled "runner_orders_cleaned" that will replace the messy values with "" and will standardize the values in the distance and duration columns. 

````sql
CREATE Table runner_orders_cleaned AS
SELECT 
order_id, 
runner_id,
CASE 
	WHEN pickup_time LIKE 'null' THEN ''
	ELSE pickup_time
END AS pickup_time,
CASE 
	WHEN distance LIKE 'null' THEN ''
	WHEN distance LIKE '%km,' THEN TRIM('km' FROM distance)
	ELSE distance
END AS distance,
CASE 
	WHEN duration LIKE 'null' THEN ''
	WHEN duration LIKE '%minute,' THEN TRIM('minute' FROM duration)
	WHEN duration LIKE '%minutes,' THEN TRIM('minutes' FROM duration)
	WHEN duration LIKE '%mins,' THEN TRIM('mins' FROM duration)
	ELSE duration
END AS duration,
CASE 
	WHEN cancellation LIKE 'null' OR cancellation IS NULL THEN ''
	ELSE cancellation
END AS cancellation
FROM runner_orders
````
