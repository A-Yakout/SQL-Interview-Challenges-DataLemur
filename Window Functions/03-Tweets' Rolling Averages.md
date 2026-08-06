# Tweets' Rolling Averages

**Difficulty:** 🟡 Medium  
**Topic:** Window Functions 
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/rolling-average-tweets)

---

## ❓ Problem Statement
Given a table of tweet data over a specified time period, calculate the 3-day rolling average of tweets for each user.
Output the user ID, tweet date, and rolling averages rounded to 2 decimal places.

Notes:
A rolling average, also known as a moving average or running mean is a time-series technique that examines trends in data over a specified period of time.
In this case, we want to determine how the tweet count for each user changes over a 3-day period.

---

## 🧠 My Approach (Business Logic)
1. Use the `AVG()` window function to get the average PARTITIONED BY user id. 
2. Framed each window (same user id) from 2 Rows behind and the current row.

---

## 💻 SQL Solution

```sql
SELECT 
  user_id,
  tweet_date,
  ROUND(AVG(tweet_count) OVER(PARTITION BY user_id ROWS BETWEEN 2 PRECEDING AND CURRENT ROW),2) AS rolling_avg_3d
FROM tweets;
