# Data Cleaning

## Table: customer_orders
Looking at customer_orders table we can see that there are values labeled "null" and NULL values throughout the table. I will create a new table labeled "customer_orders_cleaned" that will replace the messy values with "". 

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
