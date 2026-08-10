# Card Launch Success

**Difficulty:** 🟡 Medium
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/card-launch-success)

---

## ❓ Problem Statement
Your team at JPMorgan Chase is soon launching a new credit card. You are asked to estimate how many cards you'll issue in the first month.

Before you can answer this question, you want to first get some perspective on how well new credit card launches typically do in their first month.

Write a query that outputs the name of the credit card, and how many cards were issued in its launch month.
The launch month is the earliest record in the monthly_cards_issued table for a given card. Order the results starting from the biggest issued amount.

---

## 🧠 My Approach (Business Logic)
1. Used 'ROW_NUMBER()' in inner query to get data for each card and ordered by its date .
2. Getting the first record for each card (first issued month), with its name and issued amount .
3. Ordered by Issued amount descending as required in the question .

---

## 💻 SQL Solution

```sql
SELECT 
  card_name,
  issued_amount
FROM (
SELECT 
  card_name,
  issued_amount,
  ROW_NUMBER() OVER(PARTITION BY card_name ORDER BY issue_year, issue_month ASC) rnk
FROM monthly_cards_issued
) t 
WHERE rnk = 1
ORDER BY issued_amount DESC
