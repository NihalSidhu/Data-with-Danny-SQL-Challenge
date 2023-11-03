# 🍕 Pizza Runner Case Study

## Table of Contents
- [The Problem at Hand](#the-problem-at-hand)
- [Entity Relationship Model](#entity-relationship-model)
- Solution
  - [Data Scrubbing](#data-scrubbing)
  - [Pizza Metrics](#pizza-metrics)
  - Runner and Customer Experience ~ in progress
  - Ingredient Optimisation ~ in progress
  - Pricing and Ratings ~ in progress


## The Problem at Hand
Danny has recently been inspired by local pizza businesses and has decided to open a pizza shop, right inside his home. In order to sell his pizzas to customers from all around, he wants to create a strog delivery system. Danny recruited different runners to help him deliver pizza and hence the name Pizza Runner was born.

However, with little seed money, Danny has maxed out his credit card on an app to accept deliveries from his customers. With the minimal funding left, Danny needs assistance with better directing his runners during deliveries and to optimize Pizza Runner's operations as a whole. 

Luckily, Danny has provided me with lots of data from his operations. With all the questions Danny has, I will leverage my SQL skills to help Danny's Pizza Runner become the best pizza delivery in town!

## Entity Relationship Model

Here are the relationships from the different datasets that Danny provided. 

![PizzaRunner](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/91f48537-46ce-4023-bcd6-91a566b24134)

For further information on the actual data contained in each table, you can find it [here](https://8weeksqlchallenge.com/case-study-2/)

## Data Scrubbing
Prior to solving any of the case study questions, I needed to clean up the datasets. Just from initial glance at the provided tables, I noticed that some tables contained null values and other tables had inconsistent variable usage in their columns. Below I discuss the data cleaning I needed to do to the tables in order to make progress with the questions.

### Cleaning table *customer_orders*
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

With both cleaned tables, I was then ready to dive into the questions in the case study and use these tables for some querying.

## Pizza Metrics

### 1. How many pizzas were ordered?
**Query**
````sql
SELECT
	COUNT(*) AS total_pizzas_ordered
FROM
	customer_orders_cleaned;
````
**Answer**: 14 pizzas were ordered

![A1](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/492f9ff5-26e2-4ca5-bc18-f2be9e67ae36)

### 2. How many unique customer orders were made?
**Query**
````sql
SELECT
	COUNT(DISTINCT customer_id) AS unique_customer_orders
FROM
	customer_orders_cleaned;
````
**Answer**: There were 5 unique customer orders

![A2](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/b5b0d76e-e925-498a-92ca-b42135e51930)

### 3. How many successful orders were delivered by each runner?
**Query**
````sql
SELECT
	runner_id as 'runner',
	COUNT(order_id) as 'successful_orders'
FROM
	runner_orders_cleaned
WHERE
	distance > 0
GROUP BY
	runner_id;
````
**Answer**: Runner 1 had 4 successful orders, runner 2 had 3 successful orders, runner 3 had 1 successful order

![A3](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/2e9cbadc-43ca-44ba-ab18-0ddc1512d3a9)

### 4. How many of each type of pizza was delivered?
**Query**
````sql
SELECT
	p.pizza_name,
	COUNT(c.order_id) AS total_pizzas_delivered
FROM
	customer_orders_cleaned as c
LEFT JOIN runner_orders_cleaned as r
  ON
	c.order_id = r.order_id
LEFT JOIN pizza_names as p
  ON
	c.pizza_id = p.pizza_id
WHERE
	r.distance > 0
GROUP BY
	p.pizza_name;
````
**Answer**: There were 9 meatlovers pizzas delivered and 3 vegetarian pizzas delivered

![A4](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/9542952f-ea50-4b66-8a79-373c51e69ff4)

### 5. How many Vegetarian and Meatlovers were ordered by each customer?
**Query**
````sql
SELECT
	customer_id,
	p.pizza_name as pizza_type,
	COUNT(p.pizza_id) as pizzas_ordered
FROM
	customer_orders_cleaned AS c
LEFT JOIN pizza_names AS p
ON
	c.pizza_id = p.pizza_id
GROUP BY
	c.customer_id,
	p.pizza_name;
````
**Answer** Solution provided in table below

![A5](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/df7060d1-c720-4c6a-b8c6-e63bd00943f8)

### 6. What was the maximum number of pizzas delivered in a single order?
**Query**
````sql
SELECT
	order_id,
	MAX(pizzas_delivered) AS maximum_pizzas_delivered
FROM
	(
	SELECT
		c.order_id,
		COUNT(c.pizza_id) AS pizzas_delivered
	FROM
		customer_orders_cleaned AS c
	LEFT JOIN runner_orders_cleaned AS r
ON
		c.order_id = r.order_id
	WHERE
		distance > 0
	GROUP BY
		c.order_id
)
````
**Answer**: The maximum number of pizzas delivered in a single order was 3

![A6](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/bd25d655-4cb8-4111-a435-66aeb802396a)

### 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
**Query**
````sql
SELECT
	c.customer_id,
	SUM(
    CASE WHEN c.exclusions <> '' OR c.extras <> '' THEN 1
    ELSE 0
    END) AS at_least_1_change,
	SUM(
    CASE WHEN c.exclusions = '' AND c.extras = '' THEN 1 
    ELSE 0
    END) AS no_change
FROM
	customer_orders_cleaned AS c
JOIN runner_orders_cleaned AS r
  ON
	c.order_id = r.order_id
WHERE
	r.distance > 0
GROUP BY
	c.customer_id;
````
**Answer**: Solution provided in table below

![A7](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/8bdbdef1-5d94-49de-92af-828b9fa85dc7)

### 8. How many pizzas were delivered that had both exclusions and extras?
**Query**
````sql
SELECT
	COUNT(c.order_id) AS pizzas_delivered_with_exclusions_and_extras
FROM
	customer_orders_cleaned AS c
JOIN runner_orders_cleaned AS r
  ON
	c.order_id = r.order_id
WHERE
	r.distance > 0
	AND c.exclusions <> ''
	AND c.extras <> '';
````
**Answer**: Only 1 pizza was delivered that had both exclusions and extras

![A8](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/fe356649-4409-4896-9f98-6514961e28d4)

### 9. What was the total volume of pizzas ordered for each hour of the day?
**Query**
````sql
SELECT
	DATEPART(HOUR, [order_time]) AS hour_time,
	COUNT(order_id) AS pizza_volume
FROM
	customer_orders_cleaned
GROUP BY
	hour_time;
````
**Answer**: Solution provided in table below

![A9](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/ae485ad0-0bbb-48c0-a25f-2b454d7f3885)

### 10. What was the volume of orders for each day of the week?
**Query**
````sql
SELECT
	DATEPART(WEEKDAY, order_time) as days,
	COUNT(order_id) AS pizza_volume
FROM
	customer_orders_cleaned
GROUP BY
	days;
````
**Answer**: Solution provided in table below

![A10](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/d10c4ca5-a271-4b1f-b392-7ee646b06ef9)





