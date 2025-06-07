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
``SELECT population FROM world WHERE name = 'Germany';``

### 2. Población de Escandinavia
``SELECT name, population FROM world WHERE name IN ('Sweden', 'Norway', 'Denmark');``

### 3. Área entre 200,000 y 250,000
``SELECT name, area FROM world WHERE area BETWEEN 200000 AND 250000;``

## SELECT Basics Quiz

### 1. Población entre 1M y 1.25M
``SELECT name, population FROM world WHERE population BETWEEN 1000000 AND 1250000;``

### 2. Nombre comienza con "Al"
``SELECT name, population FROM world WHERE name LIKE 'Al%';``

### 3. Nombre comienza con "a" o "l"
``SELECT name FROM world WHERE name LIKE 'a%' OR name LIKE 'l%';``

### 4. Nombres de 5 letras en Europa
``SELECT name, LENGTH(name) FROM world WHERE LENGTH(name) = 5 AND region = 'Europe';``

### 5. Doble del área
``SELECT name, area * 2 FROM world WHERE population = 64000;``

### 6. Área y población pequeñas
``SELECT name, area, population FROM world WHERE area < 50000 AND population < 10000000;``

### 7. Densidad poblacional
``SELECT name, area / population FROM world WHERE name IN ('China', 'Nigeria', 'France', 'Australia');``

## SELECT Names
### Variaciones con LIKE
``-- Comienza con "Y"``
``SELECT name FROM world WHERE name LIKE 'Y%';``
``-- Termina en "Y"``
``SELECT name FROM world WHERE name LIKE '%Y';``
``-- Contiene "x"``
``SELECT name FROM world WHERE name LIKE '%x%';``
``-- Termina con "land"``
``SELECT name FROM world WHERE name LIKE '%land';``
``-- Empieza con "C" y termina en "ia"``
``SELECT name FROM world WHERE name LIKE 'C%ia';``
``-- Contiene "oo"``
``SELECT name FROM world WHERE name LIKE '%oo%';``
``-- Contiene "a" tres veces``
``SELECT name FROM world WHERE name LIKE '%a%a%a%';``
``-- Segundo carácter es "t"``
``SELECT name FROM world WHERE name LIKE '_t%';``
``-- Contiene patrón "o__o"``
``SELECT name FROM world WHERE name LIKE '%o__o%';``
``-- Nombres de 4 letras``
``SELECT name FROM world WHERE LENGTH(name) = 4;``


## Relaciones nombre-capital
``-- Mismo nombre que capital``
``SELECT name FROM world WHERE name = capital;``
``-- Capital termina en " City"``
``SELECT name FROM world WHERE capital LIKE '% City';``
``-- Capital contiene el nombre del país``
``SELECT capital, name FROM world WHERE capital LIKE CONCAT('%', name, '%');``
``-- Capital contiene el nombre del país y es más larga``
``SELECT capital, name FROM world WHERE capital LIKE CONCAT('%', name, '%') AND LENGTH(capital) > LENGTH(name);``

# SQL-BEECROWD
# SQL-BEECROWD

## LEVEL 1####
- ** EJERCICIO 2603 Customer Address **
```
solucion:
SELECT name, street
FROM customers
WHERE city = 'Porto Alegre';
```

- ** EJERCICIO 2607 Providers' City in Alphabetical Order **
```
solucion:
SELECT DISTINCT (city)
FROM providers
ORDER BY city ASC;
```

- ** EJERCICIO 2608 Higher and Lower Price **
```
solucion:
SELECT MAX(price), MIN(price)
FROM products;
```

- ** EJERCICIO 2615 Expanding the Business **
```
solucion:
SELECT DISTINCT(city)
FROM customers;
```

- ** EJERCICIO 2617 Provider Ajax SA **
```
solucion:
SELECT products.name, providers.name
FROM products
INNER JOIN providers ON
products.id_providers=providers.id
WHERE 
providers.name = 'Ajax SA';
```

- ** EJERCICIO 2622 Legal Person **
```
solucion:
SELECT c.name
FROM customers c
INNER JOIN legal_person lp ON c.id = lp.id_customers;
```

- ** EJERCICIO 2744 Passwords **
```
solucion:
SELECT id, password, MD5(password)
FROM account;
```

- ** EJERCICIO 2746 Viruses **
```
solucion:
SELECT REPLACE(name, 'H1', 'X') AS name
FROM virus;
```


# SQL-HACKER_RANK
# Aggregation ###

- ** Avarage Population **
```
SELECT FLOOR(AVG(POPULATION))
FROM CITY;
```
- ** Japan Population **
```
SELECT SUM(POPULATION) FROM CITY
WHERE COUNTRYCODE = 'JPN';
```
- ** Population Density Difference **
```
SELECT MAX(POPULATION) - MIN(POPULATION)
FROM CITY;
```
- ** Revising Aggregations **
```
SELECT AVG(POPULATION) FROM CITY
WHERE DISTRICT = 'California';
```
- ** Revising Aggregations - The counts Function **
```
SELECT COUNT(*) FROM CITY
WHERE POPULATION > 100000;
```
- ** Revising Aggregations - The Sum Function **
```
SELECT SUM(POPULATION) FROM CITY
WHERE DISTRICT = 'California';
```
- ** The Blunder **
```
SELECT
    CEIL(AVG(Salary) - AVG(REPLACE(SALARY, '0', '')))
FROM EMPLOYEES;
```
- ** Top Earners **
```
SELECT salary * months AS earnings, COUNT(*)
FROM Employee
GROUP BY earnings
ORDER BY earnings DESC
LIMIT 1;
```
- ** Weather Observation Station 13 **
```
SELECT 
    TRUNCATE(SUM(LAT_N), 4)
FROM
    STATION
WHERE
    LAT_N BETWEEN 38.7880 AND 137.2345;
```
- ** Weather Observation Station 14 **
```
SELECT
    TRUNCATE(MAX(LAT_N), 4)
FROM
    STATION
WHERE
    LAT_N < 137.2345;
```
- ** Weather Observation Station 15 **
```
SELECT ROUND(LONG_W, 4)
FROM STATION
WHERE LAT_N < 137.2345
ORDER BY LAT_N DESC
LIMIT 1;
```
- ** Weather Observation Station 16 **
```
SELECT 
    ROUND(MIN(LAT_N), 4)
FROM
    STATION
WHERE
    LAT_N > 38.7780;
```
- ** Weather Observation Station 17 **
```
SELECT ROUND(LONG_W, 4)
FROM STATION
WHERE LAT_N > 38.7780
ORDER BY LAT_N
LIMIT 1;
``` 
- ** Weather Observation Station 18 **
```
SELECT
    ROUND(ABS(MAX(LAT_N)  - MIN(LAT_N))
        + ABS(MAX(LONG_W) - MIN(LONG_W)), 4)
FROM 
    STATION;
```
- ** Weather Observation Station 19 **
```
SELECT
    ROUND(SQRT(
        POWER(MAX(LAT_N)  - MIN(LAT_N),  2)
      + POWER(MAX(LONG_W) - MIN(LONG_W), 2)
    ), 4)
FROM 
    STATION;
```
- ** Weather Observation Station 20 **
```
SELECT 
    ROUND(SUM(LAT_N), 2),
    ROUND(SUM(LONG_W), 2)
FROM
    STATION;
```

# BASIC JOIN ###
- ** African Cities **
```
SELECT CITY.NAME 
FROM CITY, COUNTRY
WHERE CITY.COUNTRYCODE = COUNTRY.CODE AND COUNTRY.CONTINENT = 'Africa';
```
- ** Asian Population **
```
SELECT SUM(CITY.POPULATION) 
FROM CITY, COUNTRY
WHERE CITY.COUNTRYCODE = COUNTRY.CODE AND COUNTRY.CONTINENT = 'Asia';
```
- ** Average Population of Each Continent **
```
SELECT COUNTRY.CONTINENT, FLOOR(AVG(CITY.POPULATION))
FROM CITY INNER JOIN COUNTRY
ON CITY.COUNTRYCODE = COUNTRY.CODE
GROUP BY COUNTRY.CONTINENT;
```
- ** The Report **
```
SELECT IF (S.Marks < 70, 'NULL', S.Name), G.Grade, S.Marks
FROM Students AS S, Grades AS G
WHERE S.Marks BETWEEN G.Min_Mark AND G.Max_Mark
ORDER BY G.GRADE DESC, S.NAME;
```

# BASIC SELECT ###
- ** Employee Names **
```
SELECT name FROM Employee
ORDER BY name;
```
- ** mployee Salaries **
```
SELECT name FROM Employee
WHERE salary > 2000 AND months < 10
ORDER BY employee_id;
```
- ** Higher Than 75 Marks **
```
SELECT Name FROM STUDENTS
WHERE Marks > 75
ORDER BY RIGHT(Name, 3), ID;
```
- ** Japanese Cities' Attributes **
```
SELECT * FROM CITY
WHERE COUNTRYCODE = 'JPN';
```
- ** Japanese Cities' Names **
```
SELECT NAME FROM CITY
WHERE COUNTRYCODE = 'JPN';
```
- ** Revising the Select Query I **
```
SELECT * FROM CITY
WHERE COUNTRYCODE = 'USA' AND POPULATION > 100000;
```
- ** Revising the Select Query II **
```
SELECT NAME FROM CITY
WHERE COUNTRYCODE = 'USA' AND POPULATION > 120000;
```
- ** Select All **
```
SELECT* FROM CITY;
```
- ** Select By ID **
```
SELECT * FROM CITY
WHERE ID = 1661;
```
- ** Weather Observation Station 1 **
```
SELECT CITY, STATE FROM STATION;
```
- ** Weather Observation Station 10 **
```
SELECT DISTINCT CITY FROM STATION
WHERE CITY REGEXP '[^aeiou]$';
```
- ** Weather Observation Station 11 **
```
SELECT DISTINCT CITY FROM STATION
WHERE CITY REGEXP '^[^aeiou]|[^aeiou]$';
```
- ** Weather Observation Station 12 **
```
SELECT DISTINCT CITY FROM STATION
WHERE CITY REGEXP '^[^aeiou].*[^aeiou]$';
```
- ** Weather Observation Station 3 **
```  
SELECT DISTINCT CITY FROM STATION
WHERE ID % 2 = 0;
```
## LEVEL 2

### EJERCICIO 2604 - Under 10 or Greater Than 100
```
solucion:
SELECT id, name
FROM products
WHERE price < 10 OR price > 100;
```

### EJERCICIO 2613 - Cheap Movies
```
solucion:
SELECT m.id, m.name
FROM movies m
INNER JOIN prices p ON m.id_prices = p.id
WHERE p.value < 2;
```

### EJERCICIO 2619 - Super Luxury
```
solucion:
SELECT p.name, pr.name, p.price
FROM products p
INNER JOIN providers pr ON p.id_providers = pr.id
INNER JOIN categories c ON p.id_categories = c.id
WHERE p.price > 1000 AND c.name = 'Super Luxury';
```

### EJERCICIO 2994 - How much does a Doctor earn?
```
solucion:
SELECT d.name, ROUND(SUM(a.hours * 150 + a.hours * 150 * (w.bonus / 100)), 1) AS salary
FROM doctors d
INNER JOIN attendances a ON d.id = a.id_doctor
INNER JOIN work_shifts w ON a.id_work_shift = w.id
GROUP BY d.id
ORDER BY salary DESC;
```

### EJERCICIO 3480 - Adjacent Chairs
```
solucion:
SELECT c1.queue, c1.id AS left, c2.id AS right
FROM chairs c1
JOIN chairs c2 ON c1.queue = c2.queue AND c1.id + 1 = c2.id
WHERE c1.available = TRUE AND c2.available = TRUE
ORDER BY c1.id;
```

### EJERCICIO 3481 - Classifying a Tree
```
solucion:
SELECT n.id,
    CASE 
        WHEN n.parent_id IS NULL THEN 'ROOT'
        WHEN c.id IS NULL THEN 'LEAF'
        ELSE 'INNER'
    END AS type
FROM nodes n
LEFT JOIN nodes c ON n.id = c.parent_id
GROUP BY n.id, n.parent_id
ORDER BY n.id;
```

### EJERCICIO 3482 - Followers
```
solucion:
SELECT
    CASE WHEN u1.posts < u2.posts THEN u1.name ELSE u2.name END AS user_with_fewer_posts,
    CASE WHEN u1.posts < u2.posts THEN u2.name ELSE u1.name END AS user_with_more_posts
FROM follows f1
JOIN follows f2 ON f1.follower_id = f2.followed_id AND f1.followed_id = f2.follower_id
JOIN users u1 ON f1.follower_id = u1.id
JOIN users u2 ON f1.followed_id = u2.id
WHERE u1.id < u2.id
ORDER BY CASE WHEN u1.posts < u2.posts THEN u1.id ELSE u2.id END;
```

### EJERCICIO 3483 - Second Largest and Smallest
```
solucion:
SELECT city_name, population
FROM cities
WHERE population = (
    SELECT DISTINCT population FROM cities ORDER BY population DESC LIMIT 1 OFFSET 1
)
UNION ALL
SELECT city_name, population
FROM cities
WHERE population = (
    SELECT DISTINCT population FROM cities ORDER BY population ASC LIMIT 1 OFFSET 1
)
ORDER BY population DESC;
```

## LEVEL 3

### EJERCICIO 2606 - Categories
```
solucion:
SELECT p.id, p.name
FROM products p
JOIN categories c ON p.id_categories = c.id
WHERE c.name LIKE 'super%';
```

### EJERCICIO 2610 - Average Value of Products
```
solucion:
SELECT ROUND(AVG(price), 2) AS price
FROM products;
```

### EJERCICIO 2618 - Imported Products
```
solucion:
SELECT p.name, pr.name, c.name
FROM products p
INNER JOIN providers pr ON p.id_providers = pr.id
INNER JOIN categories c ON p.id_categories = c.id
WHERE pr.name = 'Sansul SA' AND c.name = 'Imported';
```

### EJERCICIO 2620 - Orders in First Half
```
solucion:
SELECT c.name, o.id
FROM customers c
INNER JOIN orders o ON c.id = o.id_customers
WHERE EXTRACT(MONTH FROM o.orders_date) <= 6;
```

### EJERCICIO 2621 - Amounts Between 10 and 20
```
solucion:
SELECT pr.name
FROM providers p
INNER JOIN products pr ON p.id = pr.id_providers
WHERE (pr.amount >= 10 AND pr.amount <= 20) AND p.name LIKE 'P%';
```

### EJERCICIO 2624 - Number of Cities per Customers
```
solucion:
SELECT COUNT(DISTINCT(city))
FROM customers;
```
### EJERCICIO 2743 - Number of Characters
```
solucion:
SELECT name, LENGTH(name)
FROM people
ORDER BY LENGTH(name) DESC;
```

### EJERCICIO 2745 - Taxes
```
solucion:
SELECT name, ROUND(salary * 0.10, 2) AS tax
FROM people
WHERE salary > 3000;
```

### EJERCICIO 2993 - Most Frequent
```
solucion:
SELECT amount
FROM value_table
GROUP BY amount
ORDER BY COUNT(amount) DESC
LIMIT 1;
```

## LEVEL 4

### EJERCICIO 2602 - Basic Select
```
solucion:
SELECT name
FROM customers
WHERE state = 'RS';
```

### EJERCICIO 2605 - Executive Representatives
```
solucion:
SELECT p.name, pr.name
FROM products p
INNER JOIN providers pr ON p.id_providers = pr.id
INNER JOIN categories c ON p.id_categories = c.id
WHERE c.id = 6;
```

### EJERCICIO 2611 - Action Movies
```
solucion:
SELECT m.id, m.name
FROM movies m
JOIN genres g ON m.id_genres = g.id
WHERE g.description = 'Action';
```

### EJERCICIO 2623 - Categories with Various Products
```
solucion:
SELECT products.name, categories.name
FROM products
INNER JOIN categories ON products.id_categories = categories.id
WHERE products.amount > 100 AND categories.id IN (1,2,3,6,9)
ORDER BY categories.id ASC;
```

### EJERCICIO 2625 - CPF Validation
```
solucion:
SELECT 
  LPAD(SUBSTRING(cpf, 1, 3), 3, '0') || '.' ||
  LPAD(SUBSTRING(cpf, 4, 3), 3, '0') || '.' ||
  LPAD(SUBSTRING(cpf, 7, 3), 3, '0') || '-' ||
  LPAD(SUBSTRING(cpf, 10, 2), 2, '0') AS CPF
FROM natural_person;
```

### EJERCICIO 2738 - Contest
```
solucion:
SELECT c.name,
       ROUND(((s.math * 2) + (s.specific * 3) + (s.project_plan * 5)) / 10.0, 2) AS avg
FROM candidate c
JOIN score s ON c.id = s.candidate_id
ORDER BY avg DESC;
```

### EJERCICIO 2988 - Cearense Championship
```
solucion:
SELECT t.name,
       COUNT(m.id) AS matches,
       SUM(CASE WHEN (t.id = m.team_1 AND m.team_1_goals > m.team_2_goals)
                 OR (t.id = m.team_2 AND m.team_2_goals > m.team_1_goals)
           THEN 1 ELSE 0 END) AS victories,
       SUM(CASE WHEN (t.id = m.team_1 AND m.team_1_goals < m.team_2_goals)
                 OR (t.id = m.team_2 AND m.team_2_goals < m.team_1_goals)
           THEN 1 ELSE 0 END) AS defeats,
       SUM(CASE WHEN m.team_1_goals = m.team_2_goals THEN 1 ELSE 0 END) AS draws,
       SUM(CASE WHEN (t.id = m.team_1 AND m.team_1_goals > m.team_2_goals)
                 OR (t.id = m.team_2 AND m.team_2_goals > m.team_1_goals)
           THEN 3 WHEN m.team_1_goals = m.team_2_goals THEN 1 ELSE 0 END) AS score
FROM teams t
JOIN matches m ON t.id = m.team_1 OR t.id = m.team_2
GROUP BY t.name
ORDER BY score DESC;
```

### EJERCICIO 2990 - Employees CPF
```
solucion:
SELECT e.cpf, e.enome, d.dnome
FROM empregados e
JOIN departamentos d ON e.dnumero = d.dnumero
WHERE e.cpf NOT IN (SELECT cpf_emp FROM trabaja)
ORDER BY e.cpf;
```

