# Odd and Even Measurements

**Difficulty:** 🟡 Medium  
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/odd-even-measurements)

---

## ❓ Problem Statement
Assume you're given a table with measurement values obtained from a Google sensor over multiple days with measurements taken multiple times within each day.

Write a query to calculate the sum of odd-numbered and even-numbered measurements separately for a particular day and display the results in two different columns. 
Refer to the Example Output below for the desired format.

Definition:
Within a day, measurements taken at 1st, 3rd, and 5th times are considered odd-numbered measurements,
and measurements taken at 2nd, 4th, and 6th times are considered even-numbered measurements.

---

## 🧠 My Approach (Business Logic)
1. Used 'ROW_NUMBER()' in inner query to rank the readings in each day.
2. Used 'DATE_TRUNC()' to get the date based on the day, then grouped by that.
3. Getting SUM of values of even measuremnts by rnk % 2 = 0 and the odd measurements by rnk % 2 = 1 in the outer query.

---

## 💻 SQL Solution

```sql
SELECT 
  DATE_TRUNC('DAY', measurement_time) as measurement_day,
  SUM(CASE WHEN rnk % 2 = 1 THEN measurement_value ELSE 0 END) AS odd_sum,
  SUM(CASE WHEN rnk % 2 = 0 THEN measurement_value ELSE 0 END) AS even_sum
FROM 
(
SELECT 
  *,
  ROW_NUMBER() OVER(PARTITION BY DATE_TRUNC('DAY', measurement_time) ORDER BY measurement_id) rnk
FROM measurements
) t 
GROUP BY DATE_TRUNC('DAY', measurement_time)
ORDER BY measurement_day
