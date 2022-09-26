## Case Study #1 - Danny's Diner - Solution
### 1. What is the total amount each customer spent at the restaurant?
#### SQL Query
````sql
SELECT
  dds.customer_id,
  sum(ddm.price) as total_amount_spent
FROM dannys_diner.sales AS dds
JOIN dannys_diner.menu AS ddm 
	ON ddm.product_id = dds.product_id
GROUP BY 1
ORDER BY 1;
````
#### Answer

| customer_id | total_amount_spent |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

<hr>

### 2. How many days has each customer visited the restaurant?
#### SQL Query
````sql
SELECT
  dds.customer_id,
  count(distinct(dds.order_date)) AS days_visited
-- distinct to avoid repeating
FROM dannys_diner.sales AS dds
GROUP BY 1
ORDER BY 1;
````
#### Answer
| customer_id | days_visited |
| ----------- | ----------- |
| A           | 4          |
| B           | 6          |
| C           | 2          |
<hr>

### 3. What was the first item from the menu purchased by each customer?
#### SQL Query
````sql
-- Temporary table for new column based on order date
WITH temp_table AS (

SELECT
	dds.customer_id,  
  	dds.order_date,
  	ddm.product_name,
  	DENSE_RANK() OVER(PARTITION BY dds.customer_id 
	ORDER BY dds.order_date) AS rank
FROM dannys_diner.sales AS dds
JOIN dannys_diner.menu AS ddm
	ON dds.product_id = ddm.product_id
)

SELECT
	customer_id,
    	product_name
FROM temp_table
WHERE rank = 1
GROUP BY 1, 2;
````
#### Answer

| customer_id | product_name |
| ----------- | ----------- |
| A           | curry          |
| A           | sushi          |
| B           | curry          |
| C           | ramen          |
<hr>

### 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
#### SQL Query
````sql
SELECT
  	ddm.product_name,
  	count(dds.product_id) as purchase_count
FROM dannys_diner.menu as ddm
JOIN dannys_diner.sales as dds
ON ddm.product_id = dds.product_id
GROUP BY 1
ORDER BY 2 desc
LIMIT 1;
````
#### Answer
| product_name | purchase_count |
| ----------- | ----------- |
| ramen       | 8          |
<hr>

### 5. Which item was the most popular for each customer?
#### SQL Query
````sql
WITH temp_table AS (

SELECT
  	dds.customer_id,
	ddm.product_name,
  	count(ddm.product_id) as order_count,
  	DENSE_RANK() OVER(PARTITION BY dds.customer_id 
	ORDER BY count(dds.customer_id) DESC) as rank
FROM dannys_diner.menu as ddm
JOIN dannys_diner.sales as dds
ON ddm.product_id = dds.product_id
GROUP BY 1, 2
)

SELECT customer_id, product_name, order_count
FROM temp_table
WHERE rank = 1;
````
#### Answer
| customer_id | product_name | order_count |
| ----------- | ----------- | ----------- |
| A           | ramen          |3	|
| B           | ramen          |2	| 
| B           | curry          |2	|
| B           | sushi          |2	|
| C           | ramen          |3	|

<hr>

### 6. Which item was purchased first by the customer after they became a member?
#### SQL Query
````sql
WITH temp_table AS (

SELECT
  	dds.customer_id,
	ddm2.join_date,
  	dds.order_date,
	dds.product_id,
	DENSE_RANK() OVER(PARTITION BY dds.customer_id 
    ORDER BY dds.order_date) as rank
FROM dannys_diner.sales as dds
JOIN dannys_diner.members as ddm2
ON ddm2.customer_id = dds.customer_id
WHERE dds.order_date >= ddm2.join_date

  )
  
SELECT tt.customer_id, tt.order_date, ddm.product_name
FROM temp_table as tt
JOIN dannys_diner.menu as ddm
ON tt.product_id = ddm.product_id
WHERE rank = 1
ORDER BY 1;
````
#### Answer

| customer_id | order_date | product_name |
| ----------- | ----------- | ----------- |
| A           | 2021-01-07  | curry	|
| B           | 2021-01-11  | sushi  | 


<hr>

### 7. Which item was purchased just before the customer became a member?
#### SQL Query
#### Answer
<hr>

### 8. What is the total items and amount spent for each member before they became a member?
#### SQL Query
#### Answer
<hr>

### 9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
#### SQL Query
#### Answer
<hr>

### 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January
#### SQL Query
#### Answer
<hr>
