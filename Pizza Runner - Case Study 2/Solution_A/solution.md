## Case Study #1 - Danny's Diner - Solution
### 1. How many pizzas were ordered?
#### SQL Query
````sql
SELECT COUNT(prco.order_id) AS order_amount
FROM pizza_runner.customer_orders AS prco
````
#### Answer

| order_amount | 
| ----------- | 
| 14           | 

<hr>

### 2. How many unique customer orders were made?
#### SQL Query
````sql
SELECT COUNT(DISTINCT prco.order_id) AS unique_order_amount
FROM pizza_runner.customer_orders AS prco
````
#### Answer

| unique_order_amount | 
| ----------- | 
| 10           |

### 3. How many successful orders were delivered by each runner?
#### SQL Query
````sql
SELECT prro.runner_id,
	COUNT(prro.order_id) as successful_orders
FROM pizza_runner.runner_orders AS prro
WHERE prro.pickup_time IS NOT NULL
GROUP BY 1
ORDER BY 1;
````
#### Answer

| runner_id | successful_orders |
| ----------- | ----------- |
| 1           | 4          |
| 2           | 3          |
| 3           | 1          |

### 4. How many of each type of pizza was delivered?
#### SQL Query

#### Answer

### 5. How many Vegetarian and Meatlovers were ordered by each customer?
#### SQL Query

#### Answer

### 6. What was the maximum number of pizzas delivered in a single order?
#### SQL Query

#### Answer

### 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
#### SQL Query

#### Answer

### 8. How many pizzas were delivered that had both exclusions and extras?
#### SQL Query

#### Answer

### 9. What was the total volume of pizzas ordered for each hour of the day?
#### SQL Query

#### Answer

### 10. What was the volume of orders for each day of the week?
#### SQL Query

#### Answer
