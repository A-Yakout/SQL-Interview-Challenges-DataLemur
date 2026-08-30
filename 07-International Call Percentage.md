# International Call Percentage

**Difficulty:** 🟡 Medium

**Link:** [View Problem on DataLemur](https://datalemur.com/questions/international-call-percentage)

---

## ❓ Problem Statement
A phone call is considered an international call when the person calling is in a different country than the person receiving the call.
What percentage of phone calls are international? 
Round the result to 1 decimal.

Assumption:
The caller_id in phone_info table refers to both the caller and receiver.
---

## 🧠 My Approach (Business Logic)
1. Used the approach of double joining to get the country of the caller, then the country of the receiver .
2. Used conditional aggregation to get the Count of calls where the caller's country != the receiver's country .

---

## 💻 SQL Solution

```sql
SELECT 
  ROUND(
      100.0 * SUM(CASE WHEN c.country_id != r.country_id THEN 1 ELSE 0 END) 
      / COUNT(*)
    , 1) AS international_calls_pct
FROM phone_calls p 
LEFT JOIN phone_info c 
ON p.caller_id = c.caller_id 
LEFT JOIN phone_info r
ON p.receiver_id = r.caller_id
