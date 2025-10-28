# BRIGHT-COFFEE-CASE-STUDY

SELECT*
FROM BRIGHT_COFFEE.SALES.DATA;

-- -----------------------------------------------------

--To check my categorical lolumns
SELECT DISTINCT store_location
FROM BRIGHT_COFFEE.SALES.DATA;


SELECT DISTINCT product_category
FROM BRIGHT_COFFEE.SALES.DATA;

SELECT MIN(transaction_date) AS first_operation_date
FROM BRIGHT_COFFEE.SALES.DATA;

SELECT MAX(transaction_date) AS last_operating_date
FROM BRIGHT_COFFEE.SALES.DATA;

SELECT MIN(transaction_time) AS first_opening_time
FROM BRIGHT_COFFEE.SALES.DATA;

SELECT MAX(transaction_time) AS first_closing_time
FROM BRIGHT_COFFEE.SALES.DATA;

--------------------------------------------------------


SELECT product_category,
       store_location,
       transaction_date,
       transaction_time,
    DAYNAME (transaction_date) AS day_name,
    CASE
        WHEN day_name IN ('Sat','Sun') THEN 'Weekend'
        ELSE 'Weekday'
    END AS Day_classification,
    MONTHNAME(transaction_date) AS month_name,
    transaction_time,
    CASE
        WHEN transaction_time BETWEEN '06:00:00' AND '11:59:59' THEN 'Morning'
        WHEN transaction_time BETWEEN '12:00:00' and '17:59:59' THEN 'Afternoon'
        WHEN transaction_time >='18:00:00'THEN 'Evening'
    END AS time_bucket,
    HOUR(transaction_time) AS hour_of_day,
    SUM(transaction_qty*unit_price) AS revenue,
    (SUM(transaction_qty*unit_price)/SUM(SUM(transaction_qty*unit_price)) OVER())*100 AS revenue_percentage,
    COUNT(transaction_id*product_id) AS transaction_count,
FROM BRIGHT_COFFEE.SALES.DATA
WHERE transaction_date>'2023-04-01'
GROUP BY product_category,
         store_location,
         transaction_date,
         time_bucket,
         transaction_time
ORDER BY all;

SELECT COUNT(transaction_id) AS number_of_transactions
FROM BRIGHT_COFFEE.SALES.DATA;

SELECT COUNT(DISTINCT store_id) AS number_of_shops
FROM BRIGHT_COFFEE.SALES.DATA;

SELECT store_location,
    SUM(transaction_qty*unit_price) AS revenue
FROM BRIGHT_COFFEE.SALES.DATA
GROUP BY store_location;

SELECT DISTINCT unit_price,
FROM BRIGHT_COFFEE.SALES.DATA
ORDER BY UNIT_PRICE ASC;

SELECT SUM(unit_price) AS total_revenue
FROM BRIGHT_COFFEE.SALES.DATA;

--To check HIGH PERFORMING PRODUCTS
SELECT SUM(unit_price) AS product_performance,
       product_category,
FROM BRIGHT_COFFEE.SALES.DATA
GROUP BY product_category
HAVING product_performance>50000
LIMIT 5;

--LOW PERFORMING PRODUCTS
SELECT SUM(unit_price) AS product_performance,
       product_category,
FROM BRIGHT_COFFEE.SALES.DATA
GROUP BY product_category
HAVING product_performance<=10000
LIMIT 5;
