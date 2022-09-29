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


### 6. What was the maximum number of pizzas delivered in a single order?
#### SQL Query
````sql
WITH pizza_count_cte AS
(
  SELECT 
    prco.order_id, 
    COUNT(prco.pizza_id) AS pizza_per_order
  FROM pizza_runner.customer_orders AS prco
  JOIN pizza_runner.runner_orders AS prro
    ON prco.order_id = prro.order_id
  WHERE prro.distance IS NOT NULL
  GROUP BY prco.order_id
)

SELECT 
  MAX(pizza_per_order) AS pizza_count
FROM pizza_count_cte;
````
#### Answer
| pizza_count| 
|------------| 
| 3       |   
### 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
#### SQL Query

````sql
--#SQL_SERVER
SELECT 
  prco.customer_id,
  SUM(
    CASE WHEN prco.exclusions <> ' ' OR prco.extras <> ' ' THEN 1
    ELSE 0
    END) AS at_least_1_change,
  SUM(
    CASE WHEN prco.exclusions = ' ' AND prco.extras = ' ' THEN 1 
    ELSE 0
    END) AS no_change
FROM pizza_runner.customer_orders AS prco
JOIN pizza_runner.runner_orders AS prro
  ON prco.order_id = prro.order_id
WHERE prro.distance != 0
GROUP BY prco.customer_id
ORDER BY prco.customer_id;
````
#### Answer

| customer_id| at_least_1_change | no_change |
|------------| ----------- | ----------- |
| 101        | 0  | 2          |
| 102        | 0  | 3          |
| 103        | 3  | 0          |
| 104        | 2  | 1          |
| 105        | 1  | 0          |

### 8. How many pizzas were delivered that had both exclusions and extras?
#### SQL Query
````sql
SELECT  
  SUM(
    CASE WHEN exclusions IS NOT NULL AND extras IS NOT NULL THEN 1
    ELSE 0
    END) AS pizza_count_w_exclusions_extras
FROM pizza_runner.customer_orders AS prco
JOIN pizza_runner.runner_orders AS prro
  ON prco.order_id = prro.order_id
WHERE prro.distance >= 1 
  AND exclusions <> ' ' 
  AND extras <> ' ';
````
#### Answer
| pizza_count_w_exclusions_extras| 
|------------| 
| 1        | 
### 9. What was the total volume of pizzas ordered for each hour of the day?
#### SQL Query
SELECT 
  DATEPART(HOUR, [order_time]) AS hour_of_day, 
  COUNT(order_id) AS pizza_count
FROM pizza_runner.customer_orders 
GROUP BY DATEPART(HOUR, [order_time]);
#### Answer
| hour_of_day | pizza_count |
| ----------- | ----------- |
| 11  | 1          |
| 12  | 2          |
| 13  | 3          |
| 18  | 3          |
| 19  | 1          |
| 21  | 3          |
| 23  | 1          |
### 10. What was the volume of orders for each day of the week?
#### SQL Query
````sql
SELECT 
  FORMAT(DATEADD(DAY, 2, order_time),'dddd') AS day_of_week, 
  COUNT(order_id) AS total_pizzas_ordered
FROM pizza_runner.customer_orders
GROUP BY FORMAT(DATEADD(DAY, 2, order_time),'dddd');
````
#### Answer
| day_of_week | total_pizzas_ordered |
| ----------- | ----------- |
| Friday  | 5          |
| Monday  | 5          |
| Saturday  | 3          |
| Sunday  | 1         |
