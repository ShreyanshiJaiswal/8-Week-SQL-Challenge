## Case Study 1: Danny's Diner

![Case Study 1](https://raw.githubusercontent.com/ShreyanshiJaiswal/8-Week-SQL-Challenge/main/.images/Case_Study%231.png)

---

## Table of Contents

- [Business Task](#business-task)  
- [Entity Relationship Diagram (ERD)](#entity-relationship-diagram-erd)  
- [Question and Solution](#question-and-solution)  

Please note that all the information regarding this case study has been sourced from the following link: [here](https://8weeksqlchallenge.com/case-study-1/)

---

## Business Task

In this case study, I'm helping Danny analyze customer data to understand their visit patterns, spending habits, and favorite menu items. I’ll use SQL to answer his business questions using the `sales`, `menu`, and `members` datasets.

---

## Entity Relationship Diagram (ERD)

![ERD](https://raw.githubusercontent.com/ShreyanshiJaiswal/8-Week-SQL-Challenge/main/.images/ERD_Case%231.png)

---

## Question and Solution

I’ve solved the following question on [DB Fiddle](https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/138) 💻

---

**1. What is the total amount each customer spent at the restaurant?**
```sql
SELECT 
  sales.customer_id, 
  SUM(menu.price) AS total_sales
FROM dannys_diner.sales
INNER JOIN dannys_diner.menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id ASC;
```
**Output:**
| customer\_id | total\_sales |
| ------------ | ------------ |
| A            | 76           |
| B            | 74           |
| C            | 36           |

**2. How many days has each customer visited the restaurant?**
```sql
SELECT 
  customer_id, 
  COUNT(DISTINCT order_date) AS visit_count
FROM dannys_diner.sales
GROUP BY customer_id;
```
**Output:**
| customer_id | visit_count |
|-------------|-------------|
| A           | 4           |
| B           | 6           |
| C           | 2           |

**3. What was the first item from the menu purchased by each customer?**
```sql
SELECT 
  s.customer_id, 
  s.order_date, 
  m.product_name
FROM dannys_diner.sales AS s
JOIN dannys_diner.menu AS m 
  ON s.product_id = m.product_id
WHERE s.order_date = (
  SELECT MIN(order_date)
  FROM dannys_diner.sales AS inner_s
  WHERE inner_s.customer_id = s.customer_id
)
ORDER BY s.customer_id;
```
**Output:**
| customer_id | order_date | product_name |
|-------------|------------|--------------|
| A           | 2021-01-01 | sushi        |
| A           | 2021-01-01 | curry        |
| B           | 2021-01-01 | curry        |
| C           | 2021-01-01 | ramen        |

**4. Find the most purchased item on the menu and how many times it was purchased?**
```sql
SELECT 
  menu.product_name, 
  COUNT(*) AS frequency
FROM dannys_diner.sales
JOIN dannys_diner.menu 
  ON sales.product_id = menu.product_id
GROUP BY menu.product_name
ORDER BY frequency DESC
LIMIT 1;
```
**Output:**
| product_name | frequency |
|--------------|-----------|
| ramen        | 8         |

**5. Which item was the most popular for each customer?**
```sql
WITH ranked_orders AS (
  SELECT 
    s.customer_id,
    m.product_name,
    COUNT(*) AS order_count,
    RANK() OVER (
      PARTITION BY s.customer_id 
      ORDER BY COUNT(*) DESC
    ) AS rank
  FROM sales s
  JOIN menu m ON s.product_id = m.product_id
  GROUP BY s.customer_id, m.product_name
)

SELECT customer_id, product_name, order_count
FROM ranked_orders
WHERE rank = 1;
```
**Output:**
| customer\_id | product\_name | order\_count |
| ------------ | ------------- | ------------ |
| A            | Curry         | 2            |
| B            | Sushi         | 2            |
| C            | Ramen         | 3            |

**6. Which item was purchased first by the customer after they became a member?**
```sql

```
