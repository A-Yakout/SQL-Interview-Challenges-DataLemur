# Spotify Streaming History

**Difficulty:** 🟡 Medium
**Topic** CTE
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/spotify-streaming-history)

---

## ❓ Problem Statement
You're given two tables containing data on Spotify users' streaming activity: songs_history which has historical streaming data, and songs_weekly which has data from the current week.

Write a query that outputs the user ID, song ID, and cumulative count of song plays up to August 4th, 2022, sorted in descending order.

Assume that there may be new users or songs in the songs_weekly table that are not present in the songs_history table.

Definitions:

song_weeklytable only contains data for the week of August 1st to August 7th, 2022.
songs_history table contains data up to July 31st, 2022. The query should include historical data from this table.
---

## 🧠 My Approach (Business Logic)
1. Used CTE to get the number of song plays from the weekly table, with condition that date before 4th August as required .
2. Joining the two tables with OUTER JOIN to get all the values , on two joining conditions (user id , song id) .
3. Used COALESCE to replace NULL values with 0 .

---

## 💻 SQL Solution

```sql
WITH cte_weekly AS (
SELECT 
  user_id,
  song_id,
  COUNT(*) as cnt_song_listened
FROM songs_weekly 
WHERE listen_time <= '2022-08-04 23:59:59'
GROUP BY 1,2

)
SELECT 
  COALESCE(h.user_id, w.user_id) AS user_id,
  COALESCE(h.song_id, w.song_id) AS song_id,
  COALESCE(song_plays,0) +  COALESCE(cnt_song_listened,0) as song_plays
FROM songs_history h 
FULL OUTER JOIN cte_weekly w 
ON h.user_id = w.user_id AND h.song_id = w.song_id
ORDER BY song_plays DESC
