# Pizza Runner Case Study

## Solution to Part A - Pizza Metrics

### 1. How many pizzas were ordered?
**Query**
````sql
SELECT COUNT(*)  FROM customer_orders;
````
**Answer**

### 2. How many unique customer orders were made?
**Query**
````sql
SELECT COUNT(DISTINCT customer_id) AS unique_orders FROM customer_orders;
````
**Answer**

### 3. How many successful orders were delivered by each runner?
**Query**
````sql
SELECT 
runner_id as 'runner',
COUNT(order_id) as 'successful_orders'
FROM runner_orders
WHERE distance NOT LIKE "null"
GROUP BY runner_id;
````
**Answer**

### 4. How many of each type of pizza was delivered?
**Query**
````sql
````
**Answer**

### 5. How many Vegetarian and Meatlovers were ordered by each customer?
**Query**
````sql

````
**Answer**

### 6. What was the maximum number of pizzas delivered in a single order?
**Query**
````sql

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
