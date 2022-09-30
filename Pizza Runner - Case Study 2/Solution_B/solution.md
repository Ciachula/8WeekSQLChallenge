## Case Study #1 - Danny's Diner - Solution
### 1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
#### SQL Query
````sql
SELECT 
  DATEPART(WEEK, registration_date) AS registration_week,
  COUNT(runner_id) AS runner_signup
FROM runners
GROUP BY DATEPART(WEEK, registration_date);
````
#### Answer
| registration_week | runner_signup |
| ----------- | ----------- |
| 1           | 2          |
| 2           | 1          |
| 3           | 1          |
<hr>

### 2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?
#### SQL Query
````sql
WITH time_taken_cte AS
(
  SELECT 
    prco.order_id, 
    prco.order_time, 
    prro.pickup_time, 
    DATEDIFF(MINUTE, prco.order_time, prro.pickup_time) AS pickup_minutes
  FROM pizza_runner.customer_orders AS prco
  JOIN pizza_runner.runner_orders AS prro
    ON prco.order_id = prro.order_id
  WHERE prro.distance != 0
  GROUP BY prco.order_id, prco.order_time, prro.pickup_time
)

SELECT 
  AVG(pickup_minutes) AS avg_pickup_minutes
FROM time_taken_cte
WHERE pickup_minutes > 1;
````
#### Answer
| avg_pickup_minutes | 
| ----------- | 
| 15 | 

<hr>

### 3. Is there any relationship between the number of pizzas and how long the order takes to prepare?
#### SQL Query
````sql
WITH prep_time_cte AS
(
  SELECT 
    prco.order_id, 
    COUNT(prco.order_id) AS pizza_order, 
    prco.order_time, 
    prro.pickup_time, 
    DATEDIFF(MINUTE, prco.order_time, prro.pickup_time) AS prep_time_minutes
  FROM pizza_runner.customer_orders AS prco
  JOIN pizza_runner.runner_orders AS prro
    ON prco.order_id = prro.order_id
  WHERE prro.distance != 0
  GROUP BY prco.order_id, prco.order_time, prro.pickup_time
)

SELECT 
  pizza_order, 
  AVG(prep_time_minutes) AS avg_prep_time_minutes
FROM prep_time_cte
WHERE prep_time_minutes > 1
GROUP BY pizza_order;
````
#### Answer
| pizza_order | avg_prep_time_minutes |
| ----------- | ----------- |
| 1           | 12          |
| 2           | 16         |
| 3           | 30         |
<hr>

### 4. What was the average distance travelled for each customer?
#### SQL Query
````sql
SELECT 
	prco.customer_id, 
	SUM(prco.pizza_id) AS pizza_sum,
	AVG(prro.distance) AS avg_distance
FROM pizza_runner.customer_orders as prco
JOIN pizza_runner.runner_orders as prro 
ON prco.order_id = prro.order_id
WHERE prro.distance IS NOT NULL
GROUP BY 1
ORDER BY 1;
````
#### Answer
| customer_id | pizza_sum |avg_distance |
| ----------- | ----------- | ----------- |
| 101           | 2          |20
| 102           | 4   | 16,(3)
| 103           | 4         | 23
| 104           | 3        | 10
| 105           | 2        | 25
<hr>

### 5. What was the difference between the longest and shortest delivery times for all orders?
#### SQL Query

#### Answer

<hr>

### 6. What was the average speed for each runner for each delivery and do you notice any trend for these values?
#### SQL Query

#### Answer

<hr>

### 7. What is the successful delivery percentage for each runner?
#### SQL Query

#### Answer

