# Query Examples.
Below are some basic queries created for this project to demonstrate basic knowledge and understanding of SQL.
___
## Creating INSERT statements.
```
INSERT into Category (category_id, name, last_update) VALUES (17, 'Thriller', CURRENT_TIMESTAMP); 
INSERT into Category (category_id, name, last_update) VALUES (18, 'Crime', CURRENT_TIMESTAMP); 
INSERT into Category (category_id, name, last_update) VALUES (19, 'Mystery', CURRENT_TIMESTAMP); 
INSERT into Category (category_id, name, last_update) VALUES (20, 'Romance', CURRENT_TIMESTAMP); 
INSERT into Category (category_id, name, last_update) VALUES (21, 'War', CURRENT_TIMESTAMP)
```
These inserts create a new Category ID, Name and insert when they were added to the Category table.
___
## Updating a Column Value.
```
UPDATE film_category SET category_id = '17' WHERE film_id = '5';
SELECT * FROM film_category WHERE film_id = 5
```
This updates a columns value based off of the film_id column value, while querying for the film ID to check if it updated.
___
## Deleting a column value.
```
DELETE FROM category
WHERE NAME = 'Mystery';
SELECT * FROM category
```
This deletes a column value from the table while querying everything from the table to check if it was removed.
___
## Sorting Data.
```
SELECT * FROM film
ORDER BY title ASC, release_year DESC
```
This returns everything in the film table but sorts it by Title A-Z and latest to oldest release year.
___
## Grouping Data.
```
SELECT rating, AVG(rental_rate), MIN(rental_duration), MAX(rental_duration) FROM film
GROUP BY rating
```
This query returns the Average Rental Rate and the minimum and maximum rental duration, grouped by rating.
___
## Gathering Statistical Data.
```
SELECT rating,
COUNT (film_id)			AS "Count of Movies",
AVG(rental_rate)		AS "Average Movie Rental Rate",
MIN(rental_duration)	AS "Minimum Rental Duration",
MAX(rental_duration)	AS "Maximum Rental Duration"
FROM film
WHERE rating in ('PG','G')
ORDER BY rating GROUP by rating ASC
```
This query pulls two ratings from the table, along with the count of films, the average rental rate, the minimum and maximum rental duration, sorted by the rating.
