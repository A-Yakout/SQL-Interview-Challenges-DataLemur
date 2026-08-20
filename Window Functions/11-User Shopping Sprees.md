# User Shopping Sprees

**Difficulty:** 🟡 Medium
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/amazon-shopping-spree)

---

## ❓ Problem Statement
In an effort to identify high-value customers, Amazon asked for your help to obtain data about users who go on shopping sprees.
A shopping spree occurs when a user makes purchases on 3 or more consecutive days.

List the user IDs who have gone on at least 1 shopping spree in ascending order.
---

## 🧠 My Approach (Business Logic)
1. Wrote CTE to get the unique (distinct) transaction for each user in single day , to avoid getting more than one transaction in a single day
2. Used 'LEAD()' twice to get the next day and to get the next two days.
3. In the WHERE statement made sure that i get the exact next day by making sure that the difference is only one day

---

## 💻 SQL Solution

```sql
WITH unique_daily_transactions AS (
SELECT DISTINCT
  user_id,
  CAST(transaction_date AS DATE) date_new
FROM transactions 
) 
SELECT 
  user_id
FROM (
SELECT 
  user_id,
  date_new as current_day,
  LEAD(date_new) OVER(PARTITION BY user_id ORDER BY date_new) as nxt_day,
  LEAD(date_new,2) OVER(PARTITION BY user_id ORDER BY date_new) as nxt_two_days
FROM unique_daily_transactions
) t 
WHERE 
  nxt_day = current_day + INTERVAL '1 DAY' AND
  nxt_two_days = current_day + INTERVAL '2 DAY'
