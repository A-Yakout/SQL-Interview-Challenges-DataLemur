# Compressed Mode

**Difficulty:** 🟡 Medium
**Link:** [View Problem on DataLemur](https://datalemur.com/questions/alibaba-compressed-mode)

---

## ❓ Problem Statement

You're given a table containing the item count for each order on Alibaba, 
along with the frequency of orders that have the same item count. 
Write a query to retrieve the mode of the order occurrences. Additionally,
if there are multiple item counts with the same mode, the results should be sorted in ascending order.

Clarifications:
item_count: Represents the number of items sold in each order.
order_occurrences: Represents the frequency of orders with the corresponding number of items sold per order.
For example, if there are 800 orders with 3 items sold in each order, the record would have an item_count of 3 and an order_occurrences of 800.
---

## 🧠 My Approach (Business Logic)
1. Used 'RANK()' to get the highest occurrences with ranking , used rank due to the existance of 2 similar values .
2. Filtered with where to get the rank equals to one .

---

## 💻 SQL Solution

```sql
SELECT
  item_count as mode 
FROM 
(
  SELECT 
    item_count,
    order_occurrences,
    RANK() OVER(ORDER BY order_occurrences DESC) as rnk
  FROM items_per_order
) t 
WHERE rnk = 1

--OR
--Better performance using SubQuery and getting the max

SELECT 
  item_count AS mode
FROM items_per_order
WHERE order_occurrences = (
  SELECT MAX(order_occurrences) 
  FROM items_per_order
)
ORDER BY item_count ASC;
