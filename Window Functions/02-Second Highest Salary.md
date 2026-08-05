# Second Highest Salary

**Difficulty:** 🟡 Medium  
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/sql-second-highest-salary)

---

## ❓ Problem Statement
Imagine you're an HR analyst at a tech company tasked with analyzing employee salaries.
Your manager is keen on understanding the pay distribution and asks you to determine the second highest salary among all employees.

It's possible that multiple employees may share the same second highest salary. In case of duplicate, display the salary only once.

---

## 🧠 My Approach (Business Logic)
1. Use the `ROW_NUMBER()` window function to rank salary for all employees based on their salary  Descending.
2. Using Sub query to get only the rank number 2 .

---

## 💻 SQL Solution

```sql
SELECT 
  salary AS second_highest_salary
FROM 
(
SELECT 
  employee_id,
  salary,
  ROW_NUMBER() OVER(ORDER BY salary DESC) rnk
FROM employee
) t 
WHERE rnk = 2

