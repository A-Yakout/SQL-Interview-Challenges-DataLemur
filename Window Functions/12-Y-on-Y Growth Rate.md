# User Shopping Sprees

**Difficulty:** 🔴 Hard


**Topic:** Window Functions 


**Link:** [View Problem on DataLemur](https://datalemur.com/questions/yoy-growth-rate)

---

## ❓ Problem Statement
Assume you're given a table containing information about Wayfair user transactions for different products. 
Write a query to calculate the year-on-year growth rate for the total spend of each product, grouping the results by product ID.

The output should include the year in ascending order, product ID, current year's spend, previous year's spend and year-on-year growth percentage, rounded to 2 decimal places.


---

---

## 💻 SQL Solution

```sql

WITH cte AS (
SELECT 
  EXTRACT(YEAR FROM transaction_date) as year,
  product_id,
  SUM(spend) as curr_year_Spend
FROM user_transactions
GROUP BY 1,2

),cte2 as (
SELECT 
  *,
  LAG(curr_year_Spend,1) OVER(PARTITION BY product_id ORDER BY year) as prev_year_spend
FROM cte 
)
SELECT 
  *,
  ROUND((curr_year_spend - prev_year_spend) / prev_year_spend*100.0 ,2) AS yoy_rate
FROM cte2
ORDER BY product_id,year
