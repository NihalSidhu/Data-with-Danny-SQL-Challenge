# Data Cleaning

## 🔨 Table: customer_orders

````sql
UPDATE customer_orders 
SET exclusions = "" WHERE exclusions LIKE "null";
````
