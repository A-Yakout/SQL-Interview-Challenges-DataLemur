# FAANG Stock Min-Max (Part 1)

**Difficulty:** 🟡 Medium

**Link:** [View Problem on DataLemur](https://datalemur.com/questions/sql-bloomberg-stock-min-max-1)

---

## ❓ Problem Statement
The Bloomberg terminal is the go-to resource for financial professionals, offering convenient access to a wide array of financial datasets.
As a Data Analyst at Bloomberg, you have access to historical data on stock performance.

Currently, you're analyzing the highest and lowest open prices for each FAANG stock by month over the years.

For each FAANG stock, display the ticker symbol, the month and year ('Mon-YYYY') with the corresponding highest and lowest open prices (refer to the Example Output format).
Ensure that the results are sorted by ticker symbol.

---


## 💻 SQL Solution

```sql
with high as (
SELECT
  ticker,
  TO_CHAR(date, 'Mon-YYYY') AS highest_mth,
  open highest_open,
  ROW_NUMBER() OVER(PARTITION BY ticker ORDER BY open DESC) max_rank
FROM stock_prices
), low as (
SELECT
  ticker,
  TO_CHAR(date, 'Mon-YYYY') AS lowest_mth,
  open lowest_open,
  ROW_NUMBER() OVER(PARTITION BY ticker ORDER BY open ASC) min_rank
FROM stock_prices
)
SELECT 
  h.ticker,
  highest_mth,
  highest_open,
  lowest_mth,
  lowest_open
FROM high h 
JOIN low l 
ON h.ticker = l.ticker 
WHERE max_rank = 1 and min_rank = 1 
