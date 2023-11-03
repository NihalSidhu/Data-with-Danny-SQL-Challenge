# Pizza Runner Case Study

## Solution to Part A - Pizza Metrics

### 1. How many pizzas were ordered?
**Query**
````sql
SELECT
	COUNT(*) AS total_pizzas_ordered
FROM
	customer_orders_cleaned;
````
**Answer**

![A1](https://github.com/NihalSidhu/Data-with-Danny-SQL-Challenge/assets/111151666/492f9ff5-26e2-4ca5-bc18-f2be9e67ae36)

### 2. How many unique customer orders were made?
**Query**
````sql
SELECT
	COUNT(DISTINCT customer_id) AS unique_customer_orders
FROM
	customer_orders_cleaned;
````
**Answer**

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
**Answer**

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
**Answer**

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
**Answer**

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
**Answer**

### 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
**Query**
````sql

````
**Answer**

### 8. How many pizzas were delivered that had both exclusions and extras?
**Query**
````sql

````
**Answer**

### 9. What was the total volume of pizzas ordered for each hour of the day?
**Query**
````sql

````
**Answer**

### 10. What was the volume of orders for each day of the week?
**Query**
````sql

````
**Answer**
