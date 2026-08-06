# Highest-Grossing Items

**Difficulty:** 🟡 Medium  
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/sql-highest-grossing)

---

## ❓ Problem Statement
Assume you're given a table containing data on Amazon customers and their spending on products in different category,
write a query to identify the top two highest-grossing products within each category in the year 2022.
The output should include the category, product, and total spend.

---

## 🧠 My Approach (Business Logic)
1. In the inner query used the 'ROW_NUMBER()' window function to rank the products based on their sales Descending.
2. Filtered the returned rows to be in 2022 only as required.
3. The outer query filtered the rank to get the highest 2 products in sales.

---

## 💻 SQL Solution

```sql
SELECT 
  category,
  product,
  total_spend
FROM (
SELECT 
  category,
  product,
  SUM(spend) as total_spend,
  ROW_NUMBER() OVER(PARTITION BY category ORDER BY SUM(spend) DESC) rnk
FROM product_spend
WHERE DATE_PART('YEAR',transaction_date) =2022
GROUP BY 
  category,
  product
) t 
WHERE rnk <=2;
