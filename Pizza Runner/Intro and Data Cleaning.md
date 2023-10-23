# Data Cleaning

## Table: customer_orders

````sql
UPDATE customer_orders 
SET exclusions = ""
WHERE exclusions LIKE "null";
````
````sql
UPDATE customer_orders 
SET extras = "" 
WHERE extras IS NULL OR extras LIKE "null";
````
