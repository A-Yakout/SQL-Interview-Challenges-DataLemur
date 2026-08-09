# Repeated Payments

**Difficulty:** 🔴 Hard  
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/repeated-payments)

---

## ❓ Problem Statement
Sometimes, payment transactions are repeated by accident; it could be due to user error, API failure or a retry error that causes a credit card to be charged twice.
Using the transactions table, identify any payments made at the same merchant with the same credit card for the same amount within 10 minutes of each other. Count such repeated payments.

Assumptions:
The first transaction of such payments should not be counted as a repeated payment.
This means, if there are two transactions performed by a merchant with the same credit card and for the same amount within 10 minutes, there will only be 1 repeated payment.

---

## 🧠 My Approach (Business Logic)
1. Used 'LEAD()' in inner query to get the next transaction made by the same mercahnt,time and with the same amount.
2. Getting the difference in time between the current transaction and the next transaction from the lead function .
3. Getting the Count transactions that have done in the same 10 minutes .

---

## 💻 SQL Solution

```sql
SELECT 
  COUNT(transaction_id) AS payment_count
FROM(
SELECT
  transaction_id,
  transaction_timestamp,
  EXTRACT(EPOCH FROM (nxt_trnsction - transaction_timestamp))/ 60 AS gap_minutes
FROM (
SELECT 
  transaction_id,
  transaction_timestamp,
  LEAD(transaction_timestamp) OVER(PARTITION BY merchant_id, credit_card_id, amount ORDER BY transaction_timestamp) nxt_trnsction
FROM transactions
) t
)r 
WHERE gap_minutes <= 10
