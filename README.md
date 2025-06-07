# LAB_BdD##

# SQL-DATA LEMUR

## Lección 101 - SQL TUTORIAL INTRO
**SQL Select Practice Exercise**
```
SELECT user_id, stars 
FROM reviews 
WHERE stars = 3;
```
## Lección 102 - SQL SELECT

**SQL Select Practice Exercise**

```sql
SELECT * FROM Products;
```

## Lección 103 - SQL WHERE

**SQL Select Practice Exercise**

```sql
SELECT user_id, stars 
FROM reviews 
WHERE stars = 3;
```

## Lección 104 - SQL AND, OR, NOT

**SQL Select Practice Exercise**

```sql
SELECT * 
FROM reviews 
WHERE stars >= 4  
  AND review_id < 6000  
  AND review_id > 2000  
  AND NOT user_id = 142;
```


## Lección 105 - SQL BETWEEN

```sql
SELECT manufacturer, drug, units_sold 
FROM pharmacy_sales 
WHERE manufacturer IN ('Biogen', 'AbbVie', 'Eli Lilly') 
  AND units_sold BETWEEN 100000 AND 105000;
```

## Lección 106 - SQL IN

```sql
SELECT manufacturer, drug, units_sold 
FROM pharmacy_sales 
WHERE manufacturer IN ('Roche', 'Bayer', 'AstraZeneca') 
  AND units_sold NOT BETWEEN 55000 AND 550000;
```


## Lección 107 - SQL LIKE

```sql
SELECT * 
FROM customers 
WHERE customer_name LIKE 'F%ck';
```

## Lección 108 - SQL FILTERING REVIEW

```sql
SELECT * 
FROM customers 
WHERE age BETWEEN 18 AND 22 
  AND state IN ('Victoria', 'Tasmania', 'Queensland') 
  AND gender != 'n/a' 
  AND (customer_name LIKE 'A%' OR customer_name LIKE 'B%');
```


## Lección 109 - SQL ORDER BY

```sql
SELECT drug, (total_sales - cogs) AS total_profit 
FROM pharmacy_sales 
ORDER BY total_profit DESC 
LIMIT 3;
```

## Lección 202 - SUM, AVG, COUNT

```sql
SELECT COUNT(*) FROM pharmacy_sales;
SELECT COUNT(product_id), SUM(total_sales) FROM pharmacy_sales WHERE manufacturer = 'Pfizer';
SELECT AVG(open) FROM stock_prices WHERE ticker = 'GOOG';
SELECT MIN(open) FROM stock_prices WHERE ticker='MSFT';
SELECT MAX(open) FROM stock_prices WHERE ticker='NFLX';
```

## Lección 203 - SQL GROUP BY

```sql
SELECT ticker, MIN(open) FROM stock_prices GROUP BY ticker ORDER BY min DESC;
SELECT skill, COUNT(candidate_id) FROM candidates GROUP BY skill ORDER BY count DESC;
```

## Lección 204 - SQL HAVING

```sql
SELECT ticker, MIN(open) FROM stock_prices GROUP BY ticker HAVING MIN(open) > 100;
SELECT candidate_id FROM candidates GROUP BY candidate_id HAVING COUNT(candidate_id) > 2;
```

## Lección 205 - SQL DISTINCT

```sql
SELECT category, COUNT(DISTINCT product) FROM product_spend GROUP BY category;
```


## Lección 206 - SQL ARITHMETIC

```sql
SELECT drug, total_sales - cogs AS total_profit FROM pharmacy_sales ORDER BY total_profit DESC LIMIT 3;
SELECT card_name, MAX(issued_amount) - MIN(issued_amount) AS difference FROM monthly_cards_issued GROUP BY card_name ORDER BY difference DESC;
SELECT ticker, COUNT(ticker) FROM stock_prices 
WHERE (close - open)/open > 0.10 OR (close - open)/open < -0.10 
GROUP BY ticker ORDER BY count DESC;
```


## Lección 207 - SQL MATH FUNCTIONS

```sql
SELECT drug, CEIL(total_sales / units_sold) AS unit_cost 
FROM pharmacy_sales 
WHERE manufacturer = 'Merck' 
ORDER BY unit_cost;
```

## Lección 209 - SQL NULL

```sql
SELECT part, assembly_step FROM parts_assembly WHERE finish_date IS NULL;
```

## Lección 210 - SQL CASE

```sql
SELECT actor, character, platform, avg_likes, 
  CASE 
    WHEN avg_likes >= 15000 THEN 'Super Likes'
    WHEN avg_likes BETWEEN 5000 AND 14999 THEN 'Good Likes'
    ELSE 'Low Likes' 
  END AS likes_category 
FROM marvel_avengers 
ORDER BY avg_likes DESC;

SELECT 
  COUNT(*) FILTER (WHERE device_type = 'laptop') AS laptop_views,
  COUNT(*) FILTER (WHERE device_type IN ('tablet', 'phone')) AS mobile_views 
FROM viewership;
```

## Lección 211 - SQL JOINS

```sql
SELECT * FROM trades JOIN users ON trades.user_id = users.user_id;

SELECT users.city, COUNT(trades.order_id) AS total_orders 
FROM trades 
INNER JOIN users ON trades.user_id = users.user_id 
WHERE trades.status = 'Completed' 
GROUP BY users.city 
ORDER BY total_orders DESC 
LIMIT 3;

SELECT page_id FROM pages;
```

## Lección 302 - DATE FUNCTIONS

```sql
SELECT user_id, MAX(post_date::DATE) - MIN(post_date::DATE) AS days_between
FROM posts
WHERE DATE_PART('year', post_date::DATE) = 2021 
GROUP BY user_id
HAVING COUNT(post_id) > 1;

SELECT * FROM emails INNER JOIN texts ON emails.email_id = texts.email_id;
```

## Lección 303 - WINDOW FUNCTIONS

```sql
SELECT MAKE_DATE(issue_year, issue_month, 1) FROM monthly_cards_issued;

SELECT transaction_date, user_id, product_id,
  RANK() OVER (PARTITION BY user_id ORDER BY transaction_date DESC) AS transaction_rank
FROM user_transactions;
```

## Lección 304 - SQL LEAD & LAG

```sql
SELECT *, 
  LEAD(CLOSE) OVER (ORDER BY date), 
  close - LEAD(CLOSE) OVER (ORDER BY date), 
  LAG(CLOSE, 3) OVER (ORDER BY date), 
  close - LAG(CLOSE, 3) OVER (ORDER BY date) AS DIFFERENCE 
FROM stock_prices 
WHERE ticker = 'GOOG';

SELECT EXTRACT(YEAR FROM u1.transaction_date) AS year, 
       u1.product_id, u1.spend AS curr_year_spend, u2.spend AS prev_year_spend, 
       ROUND((u1.spend/u2.spend *100) - 100 , 2) AS yoy_rate
FROM user_transactions u1
LEFT JOIN user_transactions u2 
  ON EXTRACT(YEAR FROM u1.transaction_date)-1 = EXTRACT(YEAR FROM u2.transaction_date)
 AND u1.product_id = u2.product_id
ORDER BY 2,1;
```

## Lección 305 - SQL SELF-JOINS

```sql
SELECT b1.genre, b1.book_title AS current_book, b2.book_title AS suggested_book 
FROM goodreads AS b1
INNER JOIN goodreads AS b2 ON b1.genre = b2.genre 
WHERE b1.book_id != b2.book_id 
ORDER BY b1.book_title;
```

## Lección 306 - SQL UNION

```sql
SELECT item_type, SUM(square_footage) AS total_sqft, COUNT(*) AS item_count 
FROM inventory GROUP BY item_type;

SELECT page_id FROM pages 
EXCEPT 
SELECT page_id FROM page_likes;
```


## Lección 310 - STRING FUNCTIONS

```sql
SELECT * FROM customers 
WHERE LOWER(customer_name) LIKE '%son' 
  AND gender = 'Male' 
  AND age = 20;
```

## Lección 311 - INSTACART SQL CASE

```sql
SELECT COUNT(*) AS total_items 
FROM (
  SELECT * FROM ic_order_products_curr 
  UNION ALL 
  SELECT * FROM ic_order_products_prior
) AS all_orders;

SELECT COUNT(DISTINCT product_id) AS unique_products 
FROM ic_products;
```
# SQL-ZOO
## SELECT Basics

### 1. Población de Alemania
sql
SELECT population FROM world WHERE name = 'Germany';


### 2. Población de Escandinavia
sql
SELECT name, population FROM world WHERE name IN ('Sweden', 'Norway', 'Denmark');


### 3. Área entre 200,000 y 250,000
sql
SELECT name, area FROM world WHERE area BETWEEN 200000 AND 250000;


## SELECT Basics Quiz

### 1. Población entre 1M y 1.25M
sql
SELECT name, population FROM world WHERE population BETWEEN 1000000 AND 1250000;


### 2. Nombre comienza con "Al"
sql
SELECT name, population FROM world WHERE name LIKE 'Al%';


### 3. Nombre comienza con "a" o "l"
sql
SELECT name FROM world WHERE name LIKE 'a%' OR name LIKE 'l%';


### 4. Nombres de 5 letras en Europa
sql
SELECT name, LENGTH(name) FROM world WHERE LENGTH(name) = 5 AND region = 'Europe';


### 5. Doble del área
sql
SELECT name, area * 2 FROM world WHERE population = 64000;


### 6. Área y población pequeñas
sql
SELECT name, area, population FROM world WHERE area < 50000 AND population < 10000000;


### 7. Densidad poblacional
sql
SELECT name, area / population FROM world WHERE name IN ('China', 'Nigeria', 'France', 'Australia');


## SELECT Names

### Variaciones con LIKE
sql
-- Comienza con "Y"
SELECT name FROM world WHERE name LIKE 'Y%';
-- Termina en "Y"
SELECT name FROM world WHERE name LIKE '%Y';
-- Contiene "x"
SELECT name FROM world WHERE name LIKE '%x%';
-- Termina con "land"
SELECT name FROM world WHERE name LIKE '%land';
-- Empieza con "C" y termina en "ia"
SELECT name FROM world WHERE name LIKE 'C%ia';
-- Contiene "oo"
SELECT name FROM world WHERE name LIKE '%oo%';
-- Contiene "a" tres veces
SELECT name FROM world WHERE name LIKE '%a%a%a%';
-- Segundo carácter es "t"
SELECT name FROM world WHERE name LIKE '_t%';
-- Contiene patrón "o__o"
SELECT name FROM world WHERE name LIKE '%o__o%';
-- Nombres de 4 letras
SELECT name FROM world WHERE LENGTH(name) = 4;


## Relaciones nombre-capital
sql
-- Mismo nombre que capital
SELECT name FROM world WHERE name = capital;
-- Capital termina en " City"
SELECT name FROM world WHERE capital LIKE '% City';
-- Capital contiene el nombre del país
SELECT capital, name FROM world WHERE capital LIKE CONCAT('%', name, '%');
-- Capital contiene el nombre del país y es más larga
SELECT capital, name FROM world WHERE capital LIKE CONCAT('%', name, '%') AND LENGTH(capital) > LENGTH(name);

# SQL-BEECROWD
# SQL-HACKER_RANK
