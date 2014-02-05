# Database Constraints

## NOT NULL

`null` is a data type that represents unknown data, rather than no data whatsoever. Since the value of each `null` is unknown, it could be anything, and thus is impossible to compare. For example, let's assume a User record has a `date_of_birth` column, and the `date_of_birth` can be `null` (unknown):

	SELECT * FROM Users WHERE date_of_birth >= '1999-01-01' OR date_of_birth <= '1999-01-01';
	
Although we would expect the statement above to select all records, it cannot select records whose `date_of_birth` is `null`, because SQL has no way to compare an unknown entity with a date.

`NOT NULL` is a constraint that does not allow us to enter `null` data in a particular field. A field must contain an entry, or it is invalid, and won't be entered into the database. This can ensure data integrity, but we'll want to use it sparingly, since certain fields may be allowed to be unknown (a user may well _not_ want to provide a social security number, and we shouldn't force them to unless we absolutely require on, for example):

	CREATE TABLE Users
	(
	id integer NOT NULL AUTO_INCREMENT Primary Key,
	name varchar(50) NOT_NULL,
	date_of_birth varchar(100)
	)
	
The table above requires an `id` (the primary key, which we should always require not to be `null`), and it requires a `name`, because we've decided all users absolutely must have names to be valid. It does not require a `date_of_birth` entry, however. We're willing to let that slide, but should be careful not to query for `date_of_birth` with the assumption that we'll find all users whose birth dates really do match a certain value; the value _could_ be `null`. 

## UNIQUE

The unique constraint can be applied to an individual field:

	CREATE TABLE Users
	(
	id integer NOT NULL UNIQUE AUTO_INCREMENT Primary Key
	);
	
In the table above, the `id`, the primary key, must always be unique. That way we can ensure we always reference the correct record using a single unique identifier.

The unique constraint can also be applied to a group of fields taken together:

	CREATE TABLE FavoriteMovies
	(
	user_id NOT NULL Primary Key,
	film_id NOT NULL Primary Key,
	CONSTRAINT unique_favorite UNIQUE(user_id, film_id)
	);
	
In the table above, a user can have many favorite films, and a film can be favorited by many users, but an individual film cannot be a user's favorite many times over. If we allowed the user to have many records for the same favorite film, and then we attempted to delete one of these favorites, other records might still exist linking that film to that user as the user's favorite. The user might be understandably upset to find that _Good Will Hunting_ is still one of their favorite movies, when they clearly said that it no longer was.

## CHECK

The `CHECK` constraint is not utilized by MySQL, but many other SQL databases do allow it. It allows us to specify arbitrary constraints for data entered into our tables, and can help us to define a lot of domain-specific information:

	CREATE TABLE Users
	(
	age integer CHECK (age >= 0)
	);
	
In the table above, we check that the user's age is a positive integer or zero. If it is not, it is not a valid age. Again, MySQL does not utilize the `CHECK` constraint, and will allow entries into the database that do not meet the constraint described (so look out).

## PRIMARY KEY

The `PRIMARY KEY `constraint is the most important, as first normal form requires each table to have a unique primary key. The primary key provides a link between tables used in associations.

	CREATE TABLE Users
	(
	id integer NOT NULL PRIMARY KEY AUTO_INCREMENT
	name varchar(50)
	);
	
	CREATE TABLE HolidayBookings
	(
	customer_id int NOT NULL,
	booking_id int NOT NULL,
	CONSTRAINT prim_key PRIMARY KEY (customer_id, booking_id)
	);
	
In the case of the join table, above, we can use the combination of two fields (`customer_id` and `booking_id` to create a unique primary key).

## FOREIGN KEYS

Foreign keys create a link between two tables. In the case of a user record that refers to a location record, rather than duplicate location information on the user, we refer to the location record via a foreign key:

	CREATE TABLE Attendances
	(
	location_id int NOT NULL,
	CONSTRAINT location_fk
	FOREIGN KEY (location_id)
	REFERENCES Locations(id)
	);
	
The foreign key constraint will ensure that a record exists for the record we're referencing, or else it will not be added to the table.


	
