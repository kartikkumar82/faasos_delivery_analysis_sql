# 🍕 Faasos Delivery Project Analysis (SQL) - Day 7

This project is part of my **"7 Days, 7 SQL Projects"** challenge, where I built one SQL-based data analysis project every day for a week.  
On **Day 7**, I analyzed Faasos delivery order data to extract insights about order trends, delivery performance, and customer behavior.

---

## 📌 Project Overview
Faasos is an Indian food delivery service. In this project, I worked with hypothetical delivery data to answer important business questions, such as:

- What are the busiest order hours?
- Which food items are most popular?
- How does delivery time vary across cities?
- What percentage of orders are delayed?
- Who are the top customers by order value?

---

## 📂 Dataset Description
The dataset contains sample delivery order details.

| Column Name       | Description |
|-------------------|-------------|
| `order_id`        | Unique order ID |
| `customer_id`     | Unique customer ID |
| `order_date`      | Date and time of the order |
| `city`            | Customer city |
| `item_name`       | Food item ordered |
| `quantity`        | Number of items ordered |
| `price`           | Price per item |
| `delivery_time`   | Delivery time in minutes |
| `order_status`    | Status (Delivered / Cancelled / Delayed) |

---

## 🛠 SQL Concepts Used
- **SELECT** & **WHERE** filtering
- **GROUP BY** & **ORDER BY**
- **Aggregate Functions** (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`)
- **JOINs** (where applicable)
- **DATE Functions** (`EXTRACT`, `DATENAME`)
- **CASE WHEN** for conditional categorization
- **Subqueries & CTEs** for complex analysis

---

## 📊 Example Insights
1. **Busiest Order Hours** – Evening (7 PM - 9 PM) had the most orders.
2. **Top-Selling Items** – Wraps & Biryani were ordered most frequently.
3. **Average Delivery Time** – ~28 minutes overall, but slower in Tier-2 cities.
4. **Late Delivery %** – Around 12% of orders were delayed.
5. **Top Customers** – Few customers contributed significantly to total revenue.

---

## 📜 Sample SQL Queries

### 1️⃣ Top 5 Selling Items
```sql
SELECT item_name, COUNT(*) AS total_orders
FROM orders
GROUP BY item_name
ORDER BY total_orders DESC
LIMIT 5;
