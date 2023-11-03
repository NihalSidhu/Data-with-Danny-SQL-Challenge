# 🍕 Pizza Runner Case Study

## Table of Contents
- [The Problem at Hand](#the-problem-at-hand)
- [Entity Relationship Model](#entity-relationship-model)
- Solution
  - [Data Scrubbing](#-data-scrubbing)
  - [A. Pizza Metrics]
  - [B. Runner and Customer Experience]
  - [C. Ingredient Optimisation]
  - [D. Pricing and Ratings]


## The Problem at Hand
TO DO

## Entity Relationship Model

![PizzaRunner](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/91f48537-46ce-4023-bcd6-91a566b24134)

# Data Scrubbing
Prior to solving any of the case study questions, I needed to clean up the datasets. Just from initial glance at the provided tables, I noticed that some tables contained null values and other tables had inconsistent variable usage in their columns. Below I discuss the data cleaning I needed to do to the tables in order to make progress with the questions.

## Cleaning table *customer_orders*
At initial glance of the customer_orders table I noticed that there were some issues with it. We can see a few issues, such as

- The 'exclusions' column has multiple values labeled as "null"
- The 'extras'  column has multiple values labeled as "null" or are actually NULL.

*customer_orders table*

![customer_orders](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/2b42cb09-925d-4d6d-a819-ea727da1a93b)

With the rogue data in these two columns, I needed to create a new cleaned version of the customer_orders. In order to do this, I looked for cells that were either 'null' or NULL and I replaced the dirty data with blanks in the new customer_orders_cleaned table. 

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

*customer_orders_cleaned table*

![customer_orders_cleaned](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/ad39e96c-feb9-4cf9-8579-017a80a86966)

With a cleaned version of customer_orders, I then moved to the other dirty table, runner_orders.

## Cleaning table *runner_orders* 
When looking over the runner_orders table, I noticed multiple key issues with the dataset. Some of the issues noticed were:

- The 'pickup_time' column has multiple values labeled as 'null'
- The 'distance' column has no consistency with its usage of 'km' and has multiple values labeled as 'null'
- The 'duration' column has no consistency with what what term should be used for minutes (mins, minute or minutes). Along with this, it has mutliple values labeled as 'null'
- The 'cancellation' column has multiple values labeled as "null" or are actually NULL

*runner_orders table*

![runner_orders](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/2bd210a9-b6b8-4c46-9806-9d6229b2e006)

With the inconsistencies across the table, I needed to create a new cleaned version of the runner_orders. To do this, I had to make the following changes in each column:
- pickup_time: Replaced "null" with blanks
- distance:  Replaced 'null' and 'km' with blanks
- duration: Replaced 'min', 'minute', 'minutes' and 'null' with blanks
- cancellation: Replaced 'null' and NULL with blanks
  
All these updates were then applied into a new table, runner_orders_cleaned.

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

*runner_orders_cleaned table*

![runner_orders_cleaned](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/d162d7d3-49f4-4d2b-a542-346f8c4e31eb)

With both cleaned tables, I was then ready to dive into the questions in the case study and use these tables for some querying. The first part, pizza metrics, can be found [here](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/blob/main/Pizza%20Runner/A.%20Pizza%20Metrics.md).
