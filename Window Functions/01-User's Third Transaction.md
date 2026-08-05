# User's Third Transaction

**Difficulty:** 🟡 Medium  
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/sql-third-transaction)

---

## ❓ Problem Statement
Assume you are given the table below on Uber transactions made by users.
Write a query to obtain the third transaction of every user. Output the user id, spend and transaction date.

---

## 🧠 My Approach (Business Logic)
1. Use the `ROW_NUMBER()` window function to rank transaction within each user based on transaction date Ascending.
2. Using Sub query to get only the rank number 3 of each user.

---

## 💻 SQL Solution

```sql
SELECT 
  user_id,
  spend,
  transaction_date
FROM(
SELECT 
  user_id,
  spend,
  transaction_date,
  ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY transaction_date) rnk
FROM transactions
) t 
WHERE rnk = 3
