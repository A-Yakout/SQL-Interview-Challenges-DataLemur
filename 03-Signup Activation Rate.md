# Signup Activation Rate

**Difficulty:** 🟡 Medium
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/signup-confirmation-rate)

---

## ❓ Problem Statement
New TikTok users sign up with their emails. They confirmed their signup by replying to the text confirmation to activate their accounts.
Users may receive multiple text messages for account confirmation until they have confirmed their new account.

A senior analyst is interested to know the activation rate of specified users in the emails table. Write a query to find the activation rate. Round the percentage to 2 decimal places.

Definitions:
emails table contain the information of user signup details.
texts table contains the users' activation information.
---

## 🧠 My Approach (Business Logic)
1. Joined the table of texts with left join to get all the users even those who didn't get any confimration email .
2. getting the count of distinct email id with signup action = confirmed, without ELSE = 0 to avoid counting the zeroes .
3. Then divide it by the total number of users from emails table

---

## 💻 SQL Solution

```sql
SELECT 
  ROUND(
    COUNT(DISTINCT CASE WHEN t.signup_action = 'Confirmed' THEN e.email_id END) * 1.0 
    / COUNT(DISTINCT e.email_id), 
    2
  ) AS activation_rate
FROM emails e
LEFT JOIN texts t 
  ON e.email_id = t.email_id;
