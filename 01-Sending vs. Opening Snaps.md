# Sending vs. Opening Snaps

**Difficulty:** 🟡 Medium
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/time-spent-snaps)

---

## ❓ Problem Statement
Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these activities grouped by age group.
Round the percentage to 2 decimal places in the output.

Notes:
Calculate the following percentages:
time spent sending / (Time spent sending + Time spent opening)
Time spent opening / (Time spent sending + Time spent opening)
To avoid integer division in percentages, multiply by 100.0 and not 100.

---

## 🧠 My Approach (Business Logic)
1. Used the idea of Conditional Aggregation to get the sum of each status (open, send) in sub query, grouped by age bucket.
2. Getting the percentage of each status in the outer query and rounding the result to 2 decimal point .

---

## 💻 SQL Solution

```sql
SELECT 
  age_bucket,
  ROUND(send_total * 100.0 / (open_total + send_total),2) send_perc,
  ROUND(open_total * 100.0 / (open_total + send_total),2) open_perc
FROM(  
SELECT 
  age_bucket,
  SUM(CASE WHEN activity_type = 'open' THEN time_spent ELSE 0 END) open_total,
  SUM(CASE WHEN activity_type = 'send' THEN time_spent ELSE 0 END) send_total
FROM activities a 
JOIN age_breakdown b 
ON a.user_id = b.user_id 
GROUP BY age_bucket
) t 
