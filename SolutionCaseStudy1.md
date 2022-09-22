## Case Study #1 - Danny's Diner - Solution
### 1. What is the total amount each customer spent at the restaurant?
#### a) SQL Query
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
#### b) Answer

| customer_id | total_amount_spent |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

<hr>

### 2. How many days has each customer visited the restaurant?
#### a) SQL Query
````sql
SELECT
  dds.customer_id,
  count(distinct(dds.order_date)) AS days_visited
-- distinct to avoid repeating
FROM dannys_diner.sales AS dds
GROUP BY 1
ORDER BY 1;
````
#### b) Answer
| customer_id | days_visited |
| ----------- | ----------- |
| A           | 4          |
| B           | 6          |
| C           | 2          |
<hr>

### 3. What was the first item from the menu purchased by each customer?
#### a) SQL Query
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
#### b) Answer

| customer_id | product_name |
| ----------- | ----------- |
| A           | curry          |
| A           | sushi          |
| B           | curry          |
| C           | ramen          |
<hr>

### 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
#### SQL Query
#### Answer
<hr>

### 5. Which item was the most popular for each customer?
#### SQL Query
#### Answer
<hr>

### 6. Which item was purchased first by the customer after they became a member?
#### SQL Query
#### Answer
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
