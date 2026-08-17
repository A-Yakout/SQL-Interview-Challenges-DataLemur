# Supercloud Customer

**Difficulty:** 🟡 Medium
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/supercloud-customer)

---

## ❓ Problem Statement
A Microsoft Azure Supercloud customer is defined as a customer who has purchased at least one product from every product category listed in the products table.

Write a query that identifies the customer IDs of these Supercloud customers.
---

## 🧠 My Approach (Business Logic)
1. Joined the table of products to get the category information .
2. Compared the number of distinct categories for each customer with sub query showing the real number of categories in the product table .

---

## 💻 SQL Solution

```sql
SELECT 
  customer_id
FROM customer_contracts c 
JOIN products p 
ON c.product_id = p.product_id
GROUP BY customer_id 
HAVING
  COUNT(DISTINCT p.product_category) = (SELECT COUNT(DISTINCT product_category) FROM products)
