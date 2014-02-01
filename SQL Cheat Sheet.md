# SQL Cheat Sheet

# Primary Keys

	CREATE TABLE Users (id integer NOT NULL AUTO_INCREMENT Primary Key);

# Rename Column

	ALTER TABLE name CHANGE old_column_name new_column_name date_type;
	
# Show Columns

	SHOW COLUMNS FROM name
	
# Show Tables

	SHOW TABLES
	
# Querying:

## DISTINCT:

	SELECT DISTINCT column FROM table
	
When querying with multiple distinct fields, you'll get all distinct records:

	SELECT DISTINCT column, id FROM table
	
Will return all the records, because each id is unique. 

## AS (aliasing):

	SELECT last_name AS surname FROM Members;
	
## BETWEEN:

	SELECT * FROM Films WHERE rating BETWEEN 3 AND 5;
	
(Greater than or equal to 3 and less than or equal to 5)
	
## LIKE:

Allows use of wildcard matchers:

	% - 1 or more characters of any kind
	_ - 1 character of any kind
	
	SELECT * FROM Members WHERE last_name LIKE 'J%';
	
## NOT:

Inverse of the current query:

	SELECT * FROM Members WHERE street NOT LIKE '% Street' AND street NOT LIKE '% Road';
	
## IN:

In takes queries on the same column and provides a group of options:

	SELECT * FROM Members WHERE city IN('Big Apple', 'Little Rock');
	
## ORDER BY:

`ORDER BY` should be the last clause in a statement.

Sorts in ascending order by default.

	SELECT * FROM Films ORDER BY year_released;
	
Multiple sorting:

	SELECT * FROM Films ORDER BY rating DESC, available_on_dvd DESC;
	
In a multiple sort, the sort orders first by the first value (rating). If there are multiples with the same rating, it then moves to the second column, and sorts each within that group by the second column. 

## Concatenation in MySQL:

Joining multiple columns together with an alias:

	SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM Members;
	
	SELECT CONCAT(...) AS 'Full Name' ...;
	
# Joins

## INNER JOIN:

	SELECT Members.first_name, Members.last_name, Films.name, Categories.category
	FROM FavoriteCategories INNER JOIN Categories
	ON FavoriteCategories.category_id = Categories.id
	INNER JOIN Members
	ON Members.id = FavoriteCategories.member_id
	INNER JOIN Films
	ON Films.category_id = Categories.id
	WHERE Members.id = 1
	ORDER BY Categories.category
	
