# Top Three Salaries

**Difficulty:** 🟡 Medium  
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/sql-top-three-salaries)

---

## ❓ Problem Statement
As part of an ongoing analysis of salary distribution within the company, your manager has requested a report identifying high earners in each department.
A 'high earner' within a department is defined as an employee with a salary ranking among the top three salaries within that department.
You're tasked with identifying these high earners across all departments.

Write a query to display the employee's name along with their department name and salary. 
In case of duplicates, sort the results of department name in ascending order,
then by salary in descending order. If multiple employees have the same salary, then order them alphabetically.

Note: Ensure to utilize the appropriate ranking window function to handle duplicate salaries effectively.

---

## 🧠 My Approach (Business Logic)
1. Used 'DENSE_RANK()' to get sharing ranking with no leavning gaps and ordered by Salary Descending.
2. Joined The department table to get the department information.
3. Getting the first 3 ranks in each department in the outer query.

---

## 💻 SQL Solution

```sql
SELECT 
  department_name,
  name,
  salary
FROM(
SELECT 
  department_name,
  name,
  salary,
  DENSE_RANK() OVER(PARTITION BY department_name ORDER BY salary DESC) rnk
FROM employee e 
JOIN department d 
ON e.department_id = d.department_id
) t 
WHERE rnk <= 3
ORDER BY 
  department_name ASC, 
  salary DESC, 
  name ASC;
  product
) t 
WHERE rnk <=2;
