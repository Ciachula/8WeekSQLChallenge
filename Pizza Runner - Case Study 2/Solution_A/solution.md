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
````sql
SELECT prpn.pizza_name,
	COUNT(prpn.pizza_id) AS successful_pizza_orders
FROM pizza_runner.pizza_names AS prpn
JOIN pizza_runner.customer_orders AS prco
ON prco.pizza_id = prpn. pizza_id
JOIN pizza_runner.runner_orders AS prro
ON prco.order_id = prro.order_id
WHERE prro.pickup_time IS NOT NULL
GROUP BY 1
ORDER BY 1;
````
#### Answer
| pizza_name | successful_pizza_orders |
| ----------- | ----------- |
| Meatlovers  | 9          |
| Vegetarian  | 3          |

### 5. How many Vegetarian and Meatlovers were ordered by each customer?
#### SQL Query
````sql
SELECT  prco.customer_id,
		prpn.pizza_name,
		COUNT(prpn.pizza_id) AS successful_pizza_orders
FROM pizza_runner.pizza_names AS prpn
JOIN pizza_runner.customer_orders AS prco
ON prco.pizza_id = prpn. pizza_id
JOIN pizza_runner.runner_orders AS prro
ON prco.order_id = prro.order_id
GROUP BY 1, 2
ORDER BY 1;
````
#### Answer
| customer_id| pizza_name | successful_pizza_orders |
|------------| ----------- | ----------- |
| 101        | Meatlovers  | 2          |
| 101        | Vegetarian  | 1          |
| 102       | Meatlovers  | 2          |
| 102       | Vegetarian  | 1          |
|  103      | Meatlovers  | 3          |
|  103      | Vegetarian  | 1          |
|   104     | Meatlovers  | 3          |
|  105      | Vegetarian  | 1          |

customer_id	pizza_name	successful_pizza_orders
101	Meatlovers	2
101	Vegetarian	1
102	Meatlovers	2
102	Vegetarian	1
103	Meatlovers	3
103	Vegetarian	1
104	Meatlovers	3
105	Vegetarian	1

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
