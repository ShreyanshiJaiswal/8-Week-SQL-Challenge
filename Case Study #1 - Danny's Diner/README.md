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
ORDER BY sales.customer_id ASC;` ``` `

| customer\_id | total\_sales |
| ------------ | ------------ |
| A            | 76           |
| B            | 74           |
| C            | 36           |

