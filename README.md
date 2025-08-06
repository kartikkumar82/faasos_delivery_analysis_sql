# 🍕 Faasos Delivery Project Analysis (SQL) - Day 7

<img src = 'image.jpeg' />

This project is part of my **"7 Days, 7 SQL Projects"** challenge, where I built one SQL-based data analysis project every day for a week.  
On **Day 7**, I performed in-depth analysis of Faasos delivery orders dataset using SQL.

---

## 📌 Project Overview
The project simulates real-world food delivery data and answers important questions such as:
- Total rolls ordered and unique customers
- Roll type distribution (Veg/Non-Veg)
- Successful delivery rates per driver
- Peak ordering times and weekdays
- Average delivery times, speeds, and distances
- Customer order customization patterns

---

## 📂 Dataset Description

**Tables Created**
1. **driver** – Driver ID and registration date  
2. **ingredients** – Ingredient details  
3. **rolls** – Veg and Non-Veg roll types  
4. **rolls_recipes** – Roll-to-ingredients mapping  
5. **driver_order** – Delivery details, distance, duration, cancellation status  
6. **customer_orders** – Order details, customization, and timestamps  

---

## 🛠 SQL Concepts Used
- `SELECT`, `WHERE`, `GROUP BY`, `ORDER BY`
- Aggregate functions: `COUNT`, `SUM`, `AVG`, `MAX`, `MIN`
- Conditional logic: `CASE WHEN`
- Joins: `INNER JOIN`
- CTEs (Common Table Expressions)
- Date and time functions: `EXTRACT`, `TO_CHAR`
- Window functions: `RANK`, `ROW_NUMBER`
- String operations: `REPLACE`, `TRIM`, `POSITION`, `LEFT`

---

## 📜 Key Questions Answered

### Basic Analysis
1. Total rolls ordered
2. Unique customer count
3. Successful deliveries per driver
4. Roll type distribution
5. Veg vs Non-Veg orders by customer
6. Maximum rolls in a single order

### Advanced Analysis
7. Orders with/without custom changes  
8. Orders with both exclusions and extras  
9. Orders by hour of day  
10. Orders by weekday  
11. Average pickup time per driver  
12. Relationship between rolls count & preparation time  
13. Average distance per customer  
14. Difference between longest & shortest delivery times  
15. Average speed per driver  
16. Successful delivery percentage per driver  

---

## Insert Data

### Driver Table
```sql
CREATE TABLE driver(driver_id integer,reg_date date); 

INSERT INTO driver(driver_id,reg_date) 
 VALUES (1,'01-01-2021'),
(2,'01-03-2021'),
(3,'01-08-2021'),
(4,'01-15-2021');

```
### Ingredients Table
```sql
CREATE TABLE ingredients(ingredients_id integer,ingredients_name varchar(60)); 

INSERT INTO ingredients(ingredients_id ,ingredients_name) 
 VALUES (1,'BBQ Chicken'),
(2,'Chilli Sauce'),
(3,'Chicken'),
(4,'Cheese'),
(5,'Kebab'),
(6,'Mushrooms'),
(7,'Onions'),
(8,'Egg'),
(9,'Peppers'),
(10,'schezwan sauce'),
(11,'Tomatoes'),
(12,'Tomato Sauce');

```

### Rolls Table
```sql
CREATE TABLE rolls(roll_id integer,roll_name varchar(30)); 

INSERT INTO rolls(roll_id ,roll_name) 
 VALUES (1	,'Non Veg Roll'),
(2	,'Veg Roll');


```

### Rolls_Recipes Table
```sql
CREATE TABLE rolls_recipes(roll_id integer,ingredients varchar(24)); 

INSERT INTO rolls_recipes(roll_id ,ingredients) 
 VALUES (1,'1,2,3,4,5,6,8,10'),
(2,'4,6,7,9,11,12');


```

### Driver_order Table
```sql
CREATE TABLE driver_order(order_id integer,driver_id integer,pickup_time time,distance VARCHAR(7),duration VARCHAR(10),cancellation VARCHAR(23));
INSERT INTO driver_order(order_id,driver_id,pickup_time,distance,duration,cancellation) 
 VALUES(1,1,'01-01-2021 18:15:34','20km','32 minutes',''),
(2,1,'01-01-2021 19:10:54','20km','27 minutes',''),
(3,1,'01-03-2021 00:12:37','13.4km','20 mins','NaN'),
(4,2,'01-04-2021 13:53:03','23.4','40','NaN'),
(5,3,'01-08-2021 21:10:57','10','15','NaN'),
(6,3,null,null,null,'Cancellation'),
(7,2,'01-08-2020 21:30:45','25km','25mins',null),
(8,2,'01-10-2020 00:15:02','23.4 km','15 minute',null),
(9,2,null,null,null,'Customer Cancellation'),
(10,1,'01-11-2020 18:50:20','10km','10minutes',null);

```
### Customer_orders Table
```sql
CREATE TABLE customer_orders(order_id integer,customer_id integer,roll_id integer,not_include_items VARCHAR(4),extra_items_included VARCHAR(4),order_date timestamp);
INSERT INTO customer_orders(order_id,customer_id,roll_id,not_include_items,extra_items_included,order_date)
values (1,101,1,'','','01-01-2021  18:05:02'),
(2,101,1,'','','01-01-2021 19:00:52'),
(3,102,1,'','','01-02-2021 23:51:23'),
(3,102,2,'','NaN','01-02-2021 23:51:23'),
(4,103,1,'4','','01-04-2021 13:23:46'),
(4,103,1,'4','','01-04-2021 13:23:46'),
(4,103,2,'4','','01-04-2021 13:23:46'),
(5,104,1,null,'1','01-08-2021 21:00:29'),
(6,101,2,null,null,'01-08-2021 21:03:13'),
(7,105,2,null,'1','01-08-2021 21:20:29'),
(8,102,1,null,null,'01-09-2021 23:54:33'),
(9,103,1,'4','1,5','01-10-2021 11:22:59'),
(10,104,1,null,null,'01-11-2021 18:34:49'),
(10,104,1,'2,6','1,4','01-11-2021 18:34:49');

```

---

## 📊 Sample Query

### 1️ Successful Orders by Driver
```sql
SELECT
    driver_id,
    COUNT(DISTINCT order_id) AS successful_orders
FROM driver_order
WHERE cancellation NOT IN ('Cancellation', 'Customer Cancellation')
GROUP BY driver_id;

```

### 1. how many rolls where ordered?
```sql
SELECT 
	COUNT(roll_id)
FROM customer_orders


```

### 2. how many unique customer orders were made ?
```sql
SELECT 
	COUNT(DISTINCT customer_id)
FROM customer_orders

```


###  3. how many sucessful orders were delivered by each driver?
```sql
SELECT
	driver_id,
	COUNT(DISTINCT order_id)
FROM driver_order
WHERE cancellation NOT IN ('Cancellation' , 'Customer Cancellation')
GROUP BY 1;

```

###  4. how many of each type of roll was delivered ?
```sql
SELECT 
	roll_id,COUNT(roll_id)
FROM 
	customer_orders
WHERE
	order_id IN
  (SELECT 
  	order_id
  	FROM
    (SELECT
    	*,
    	CASE WHEN cancellation IN ('Cancellation' , 'Customer Cancellation') THEN 'c' ELSE 'nc' END AS order_cancel_detail
    FROM driver_order ) as a
WHERE order_cancel_detail = 'nc')
GROUP BY 1;

```

###  5. how many veg and non-veg rolls were ordered by each customer?
```sql
SELECT 
	a.*, b.roll_name
FROM
	(select	
		customer_id,
		roll_id,
		COUNT(roll_id) as total_order
	from customer_orders
	GROUP BY 1,2) as a
INNER JOIN 
rolls b
ON 
	a.roll_id = b.roll_id;
```
###  6. what was the maximum number of rolls delivered in a single order
```sql
SELECT 
	*
FROM 
	(SELECT
		*,
		RANK() OVER(ORDER BY total_rolls DESC) AS rnk
	FROM
		(SELECT 
			order_id,
			customer_id ,
			COUNT(roll_id) as total_rolls
		FROM customer_orders
		GROUP BY 1,2
		ORDER BY 3 DESC) AS a)
WHERE rnk = 1;
```

### 7. for each customer, how many deliverd rolls had at least 1 change and how many had no change?
```sql
WITH temp_customer_orders AS (
    SELECT 
        order_id,
        customer_id,
        roll_id,
        CASE 
            WHEN not_include_items IS NULL OR TRIM(not_include_items) = '' 
            THEN '0' 
            ELSE not_include_items 
        END AS new_not_include_items,
        CASE 
            WHEN extra_items_included IS NULL 
                 OR TRIM(extra_items_included) = '' 
                 OR extra_items_included ILIKE 'null' 
                 OR extra_items_included ILIKE 'NaN'
            THEN '0' 
            ELSE extra_items_included 
        END AS new_extra_items_included,
        order_date
    FROM customer_orders
),

temp_driver_orders AS(
		SELECT
			order_id, 
			driver_id, 
			pickup_time,
			distance,
			duration,
			CASE WHEN cancellation in ('Cancellation', 'Customer Cancellation') 
				THEN 0 ELSE 1 END AS cancellation_table
		FROM driver_order
)
SELECT 
	customer_id, 
	change_table,
	COUNT(order_id)
FROM
(SELECT *, 
	CASE WHEN new_not_include_items = '0' AND new_extra_items_included = '0' THEN 'no change' ELSE 'change' END AS change_table
FROM
(SELECT * FROM temp_customer_orders 
	WHERE order_id IN
		(SELECT order_id FROM temp_driver_orders 
			WHERE cancellation_table !=0 )))
GROUP BY 1,2
ORDER BY 2;

```

###  8. how many rolls where deliverd that had both exclusion and extras?
```sql
WITH temp_customer_orders AS (
    SELECT 
        order_id,
        customer_id,
        roll_id,
        CASE 
            WHEN not_include_items IS NULL OR TRIM(not_include_items) = '' 
            THEN '0' 
            ELSE not_include_items 
        END AS new_not_include_items,
        CASE 
            WHEN extra_items_included IS NULL 
                 OR TRIM(extra_items_included) = '' 
                 OR extra_items_included ILIKE 'null' 
                 OR extra_items_included ILIKE 'NaN'
            THEN '0' 
            ELSE extra_items_included 
        END AS new_extra_items_included,
        order_date
    FROM customer_orders
),

temp_driver_orders AS(
		SELECT
			order_id, 
			driver_id, 
			pickup_time,
			distance,
			duration,
			CASE WHEN cancellation in ('Cancellation', 'Customer Cancellation') 
				THEN 0 ELSE 1 END AS cancellation_table
		FROM driver_order
)
SELECT 
	change_table, 
	COUNT(change_table)
FROM
(SELECT *, 
	CASE WHEN new_not_include_items != '0' AND new_extra_items_included != '0' THEN 'both excluded' ELSE 'either 1 or not' END AS change_table
FROM
(SELECT * FROM temp_customer_orders 
	WHERE order_id IN
		(SELECT order_id FROM temp_driver_orders 
			WHERE cancellation_table !=0 )))
GROUP BY 1;
```

###  9. what was the total number of rolls ordered of each hour of the day?
```sql
SELECT 
	hour_bucket,
	COUNT(hour_bucket)
FROM
(SELECT 
    *,
    CONCAT(
        CAST(EXTRACT(HOUR FROM order_date) AS VARCHAR), 
        '-', 
        CAST(EXTRACT(HOUR FROM order_date) + 1 AS VARCHAR)
    ) AS hour_bucket
FROM customer_orders)
GROUP BY 1;
```

###  10. what was the number of orders for each day of the week?
```sql
SELECT
	day_bucket,
	COUNT(order_id)
FROM
	(SELECT 
		*,
		TO_CHAR(order_date, 'DAY') AS day_bucket
	FROM customer_orders)
GROUP BY 1
ORDER BY 2 DESC;
```

###  11. what was the avg time in minute it took for each driver to arrive at the fassos HQ pickup the order?
```sql
SELECT 
    driver_id,
    AVG(diff) AS avg_mins
FROM (
    SELECT *
    FROM (
        SELECT *,
               ROW_NUMBER() OVER (PARTITION BY order_id ORDER BY diff) AS rnk
        FROM (
            SELECT 
                a.order_id,
                a.customer_id,
                a.roll_id,
                a.not_include_items,
                a.extra_items_included,
                a.order_date,
                b.driver_id,
                b.pickup_time,
                b.distance,
                b.duration,
                b.cancellation,
                (EXTRACT(EPOCH FROM (b.pickup_time - a.order_date)) / 60.0) AS diff
            FROM customer_orders a
            INNER JOIN driver_order b 
                ON a.order_id = b.order_id
            WHERE b.pickup_time IS NOT NULL
        ) sub1
    ) sub2
    WHERE rnk = 1
) final
GROUP BY driver_id;
```

###  12. IS there any relatioship between the numbers of rolls and how long the order take to prepare?

```sql
SELECT 
	order_id,
	COUNT(roll_id),
	ROUND(AVG(diff))
FROM
(SELECT 
    a.order_id,
    a.customer_id,
    a.roll_id,
    a.not_include_items,
    a.extra_items_included,
    a.order_date,
    b.driver_id,
    b.pickup_time,
    b.distance,
    b.duration,
    b.cancellation,
    ROUND((EXTRACT(EPOCH FROM ((a.order_date::date + b.pickup_time) - a.order_date)) / 60)) AS diff
FROM customer_orders a
INNER JOIN driver_order b 
    ON a.order_id = b.order_id
WHERE b.pickup_time IS NOT NULL) AS a
GROUP BY 1;
```

###  13. what was the avg distance travelled for each customer?
```sql
SELECT 
	customer_id, 
	ROUND(avg(distance)) as avg_distance
FROM
(SELECT 
    a.order_id,
    a.customer_id,
    a.roll_id,
    a.not_include_items,
    a.extra_items_included,
    a.order_date,
    b.driver_id,
    b.pickup_time,
    REPLACE(b.distance, 'km' , ' ')::float AS distance,
    b.duration,
    b.cancellation
FROM customer_orders a
INNER JOIN driver_order b 
    ON a.order_id = b.order_id
WHERE b.pickup_time IS NOT NULL) AS a
GROUP BY 1;
```


###  14. what was the differnce between the longest and shortest delievery time for all orders?
```sql
SELECT 
    MAX(duration_mins) - MIN(duration_mins) AS diff
FROM (
    SELECT 
        *,
        CAST(
            CASE 
                WHEN duration LIKE '%min%' 
                THEN LEFT(duration, POSITION('m' IN duration) - 1) 
                ELSE duration 
            END AS INTEGER
        ) AS duration_mins
    FROM driver_order
    WHERE duration IS NOT NULL
) AS a;
```

###  15. what is the avg speed for each driver for each delivery and do you notice any trend for these values?
```sql
SELECT
	*
FROM
(SELECT
	driver_id,
	distance,
	duration,
	AVG(distance/duration)  as avg_speed
FROM
	(SELECT
		driver_id,
		REPLACE
			(distance, 'km' , ' ')::float AS distance,
		CAST(
	            CASE 
	                WHEN duration LIKE '%min%' 
	                THEN LEFT(duration, POSITION('m' IN duration) - 1) 
	                ELSE duration 
	            END AS INTEGER
	        ) AS duration
FROM driver_order) AS a
GROUP BY 1,2,3) AS b
WHERE avg_speed IS NOT NULL;
```

###  16. what is the sucessfull delivery time for each driver?
```sql
SELECT
	driver_id,
	c*1.0/b AS sucessfull_percentage
FROM
(SELECT 
	driver_id,
	sum(cancellation_new) AS c,
	COUNT(driver_id) as b
FROM
(select 
	*,
	CASE WHEN cancellation ILIKE '%cancellation%' THEN 0 ELSE 1 END AS cancellation_new 
from driver_order) AS a 
GROUP BY 1
ORDER BY 1;
```







