# Query Examples.

Below are some query examples that I created for this project that demonstrates a further and advanced knowledge of SQL. 
___
## Joining Tables.
```
SELECT d.country,
COUNT (a.customer_id) AS "Number of Customers"
FROM customer a
	JOIN address b ON a.address_id = b.address_id
	JOIN city    c on b.city_id = c.city_id
	JOIN country d ON c.country_id = d.country_id
GROUP BY d.country
ORDER BY "Number of Customers" DESC
Limit 10
```

The above is an example of joining tables to return the results of the top 10 countries by customers sorted by descending counts.

___
## Sub-queries.
```
SELECT a.customer_id, a.first_name, a.last_name, d.country, c.city, SUM(e.amount) AS "Total Amount Paid"
FROM customer a
JOIN address	b ON a.address_id = b.address_id
JOIN city	c ON b.city_id = c.city_id
JOIN country	d ON c.country_id = d.country_id
JOIN payment	e ON  a.customer_id = e.customer_id
WHERE c.city IN (
	SELECT c.city
	FROM customer a
	JOIN address	b ON a.address_id = b.address_id
	JOIN city	c ON b.city_id = c.city_id
	JOIN country	d ON c.country_id = d.country_id
	WHERE d.country IN(
		SELECT country
		FROM customer a
		JOIN address	b ON a.address_id = b.address_id
		JOIN city	c ON b.city_id = c.city_id
		JOIN country	d ON c.country_id = d.country_id
		GROUP BY d.country ORDER BY COUNT (a.customer_id) DESC LIMIT 10)
	GROUP BY d.country, c.city ORDER BY COUNT (a.customer_id) DESC LIMIT 10)
GROUP BY a.customer_id, a.first_name, a.last_name, d.country, c.city ORDER BY "Total Amount Paid" DESC LIMIT 5
```

This query returns the top 5 customers within the top 10 countries, sorted descending in how much they've paid.

```
SELECT d.country, COUNT(DISTINCT a.customer_id) AS "Total Customers", COUNT(DISTINCT Top_Customer_Count) AS Top_Customer_Count
FROM customer a
JOIN address  b ON a.address_id = b.address_id
JOIN city     c ON b.city_id = c.city_id
JOIN country  d ON c.country_id = d.country_id
LEFT JOIN
	(SELECT a.customer_id, a.first_name, a.last_name, d.country, c.city, SUM(e.amount) AS "Total Amount Paid"
	FROM customer a
	JOIN address  b ON a.address_id = b.address_id
	JOIN city     c ON b.city_id = c.city_id
	JOIN country  d ON c.country_id = d.country_id
	JOIN payment  e ON  a.customer_id = e.customer_id
	WHERE c.city IN (
		SELECT c.city
		FROM customer a
		JOIN address  b ON a.address_id = b.address_id
		JOIN city     c ON b.city_id = c.city_id
		JOIN country  d ON c.country_id = d.country_id
		WHERE d.country IN(
			SELECT country
			FROM customer a
			JOIN address  b ON a.address_id = b.address_id
			JOIN city     c ON b.city_id = c.city_id
			JOIN country  d ON c.country_id = d.country_id
			GROUP BY d.country ORDER BY COUNT (a.customer_id) DESC LIMIT 10)
		GROUP BY d.country, c.city ORDER BY COUNT (a.customer_id) DESC LIMIT 10)
	GROUP BY a.customer_id, a.first_name, a.last_name, d.country, c.city ORDER BY "Total Amount Paid" DESC LIMIT 5)
AS Top_Customer_Count
ON a.customer_id = Top_Customer_Count.customer_id
GROUP BY d.country ORDER BY "Total Customers" DESC LIMIT 10
```

This query returns the top 10 countries, along with their customer count and identifies if a top paying customer is in that country.
___

## Creating CTE queries.
```
WITH top_5_cte (customer_id, first_name, last_name, country, city) AS
	(SELECT SELECT a.customer_id, a.first_name, a.last_name, d.country, c.city, SUM(e.amount) AS "Total Amount Paid"
	FROM customer a
	JOIN address  b ON a.address_id = b.address_id
	JOIN city     c ON b.city_id = c.city_id
	JOIN country  d ON c.country_id = d.country_id
	JOIN payment  e ON  a.customer_id = e.customer_id
	WHERE c.city IN (
		SELECT c.city
		FROM customer a
		JOIN address  b ON a.address_id = b.address_id
		JOIN city     c ON b.city_id = c.city_id
		JOIN country  d ON c.country_id = d.country_id
		WHERE d.country IN(
			SELECT country
			FROM customer a
			JOIN address  b ON a.address_id = b.address_id
			JOIN city     c ON b.city_id = c.city_id
			JOIN country  d ON c.country_id = d.country_id
			GROUP BY d.country ORDER BY COUNT (a.customer_id) DESC LIMIT 10)
		GROUP BY d.country, c.city ORDER BY COUNT (a.customer_id) DESC LIMIT 10)
	GROUP BY a.customer_id, a.first_name, a.last_name, d.country, c.city ORDER BY "Total Amount Paid" DESC LIMIT 5)
SELECT d.country, COUNT(DISTINCT a.customer_id), COUNT(DISTINCT top_5_cte) AS "Top Customer Count"
FROM customer a
JOIN address  b ON a.address_id = b.address_id
JOIN city     c ON b.city_id = c.city_id
JOIN country  d ON c.country_id = d.country_id
JOIN payment  e ON  a.customer_id = e.customer_id
LEFT JOIN top_5_cte ON a.customer_id = top_5_cte.customer_id
GROUP BY d.country, a.customer_id, top_5_cte ORDER BY "Top Customer Count" DESC LIMIT 10
```

This query returns the same as the one before it. The top 10 countries, along with their customer count and identifies if a top paying customer is in that country.
