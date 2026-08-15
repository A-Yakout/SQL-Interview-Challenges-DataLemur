# Histogram of Users and Purchases

**Difficulty:** 🟡 Medium
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/histogram-users-purchases)

---

## ❓ Problem Statement
Assume you're given a table on Walmart user transactions. 
Based on their most recent transaction date,
write a query that retrieve the users along with the number of products they bought.

Output the user's most recent transaction date, user ID, and the number of products, sorted in chronological order by the transaction date.
---

## 🧠 My Approach (Business Logic)
1. Used 'ROW_NUMBER()' in inner query to the rank of the transactions for each user to get the recent one later .
2. Used 'COUNT()' Window function to get the count of the products in the recent transaction for each user .
3. Filtered the ranks to get only the first rank for each user (most recent transaction).

---

## 💻 SQL Solution

```sql
SELECT
  transaction_date,
  user_id,
  purchase_count
FROM (
SELECT 
  transaction_date,
  user_id,
  COUNT(product_id) OVER(PARTITION BY user_id,transaction_date) purchase_count,
  ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY transaction_date DESC) rnk
FROM user_transactions
) t 
WHERE rnk = 1
ORDER BY transaction_date
