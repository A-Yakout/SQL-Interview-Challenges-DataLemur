# Best-Selling Product

**Difficulty:** 🟡 Medium
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/best-selling-products)

---

## ❓ Problem Statement
Write an SQL query to find the best-selling product in each product category.
If there are two or more products with the same sales quantity, go by whichever product which has the higher review rating.

Return the category name and product name in alphabetical order of the category.
---

## 🧠 My Approach (Business Logic)
1. Used 'ROW_NUMBER()' in inner query to the rank of the products in each category based on the sales quantity .
2. Ordered by the sales quantity then by the rating of each product (if they have the same sales quantity) .
3. Joined the products tables with the products_sales to get all the data we need .
4. In the Outer Query in the where condition we choose all the products that have the rank 1 only .

---

## 💻 SQL Solution

```sql
SELECT 
category_name,
product_name
FROM(
SELECT 
  category_name,
  product_name,
  ROW_NUMBER() OVER(PARTITION BY category_name ORDER BY sales_quantity desc ,rating desc) rnk
FROM products p 
JOIN product_sales s
ON p.product_id = s.product_id
) t 
WHERE rnk = 1 
ORDER BY category_name, product_name
