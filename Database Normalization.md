# Database Normalization

The purpose of database normalization is to reduce data duplication, although it can do so at the cost of efficiency. There are many Normal Forms, although many database designers rely on only the first three, as they are the most important for reducing data duplication, and do so at a minimal cost:

## First Normal Form

First normal form states that a database:

* Should be divided into tables whose records pertain to a single entity (e.g. a User, or a Location). A User might have a name, age, and email--things inextricably bound to the user's information, but not a list of their favorite movies. 
* Ensure there are no duplications (e.g. a UserMovies table could hold the relationships between a user and their favorite movies--but it would not duplicate the information of each individual movie or user; only hold the foreign keys that reference the user and the movie in question).
* Each table should have exactly one primary key--a unique number used to reference that entity.

A table in first normal form might look like:

	Users
	id integer NOT NULL AUTO_INCREMENT Primary Key,
	first_name varchar(50),
	last_name varchar(50),
	email varchar(50)
	
## Second Normal Form

A table in second normal form is in first normal form, and it contains no "partial dependencies" between columns and primary keys. For example, a table like the following is not in second normal form:

	UserFilms
	user_id integer,
	user_name varchar(50),
	film_id integer,
	film_name varchar(50)
	
The `user_name` in the table above is dependent upon the `user_id`, but not the `film_id`, and the `film_name` is dependent on the `film_id`, but not the `user_id`. In general, a table should have only one primary key. In order to put this table in second normal form, we need to create three tables:

	Users
	id integer NOT NULL AUTO_INCREMENT Primary Key,
	name varchar(50)
	
	Films
	id integer NOT NULL AUTO_INCREMENT Primary Key,
	name varchar(50)
	
	UserFilms
	user_id integer Primary Key
	film_id integer Primary Key
	
Taken together, the `user_id` and `film_id` in the UserFilms table represents a unique primary key. There can be no duplication of user films (movies linked to a user), or else we might end up with strange errors, such as deleting a user's favorite, and another copy of that record existing in the database. The UserFilms table itself does not have an `id` field acting as its primary key, as such a primary key _would_ allow for duplication between the other two fields. Furthermore, records in the UserFilms table represent relationships, not data, so in this case, the primary key is a single entity consisting of two fields in combination.

## Third Normal Form

A table is in third normal form if it is in second normal form and all non primary fields are dependent on the primary key in each table. For example, a User _could_ contain an address--fields for street, city, and zip code, but the street, city, and zip code fields would not be inextricably bound to a single user. Multiple users could live at the same location, which would represent duplication of the information. Instead, we break out the location information into its own Locations table, and reference the records via foreign keys:

	Users
	id integer NOT NULL AUTO_INCREMENT Primary Key,
	name varchar(50),
	location_id integer
	
	Locations
	id integer NOT NULL AUTO_INCREMENT Primary Key,
	street varchar(100),
	city varchar(100),
	zip_code varchar(12)
	
The `location_id` field in the Users table represents the relationship to a record in the Locations table, and will hold the primary key of that record for reference.

