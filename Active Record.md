# Active Record

Active Record is one of two libraries ([gems](http://www.google.com)) that ship with Rails that are used to access the [model layer](http://www.google.com) (the other being [Active Model](http://www.google.com)).

Active Record contains a class (named base), which classes in Rails models subclass by default. For example, writing:

		rails generate model User username:string password:string
		
Creates a file named user.rb in the app/models folder of the [Rails root](http://www.google.com). By default, this is the generated file:

		# The file inherits from ActiveRecord's base class
		class User < ActiveRecord::Base				  
			attr_accessible :username, :password
		end

By [subclassing](http://www.google.com) Base, the User class inherits all the methods defined in the Base class in Active Record.

The Base class is an [Object-Relational Mapper](http://www.google.com), meaning it [abstracts](http://www.google.com) the details of working with a relational database. Instead of writing [SQL](http://www.google.com) statements (or statements in a similar [Data Definition Language](http://www.google.com), you can write statements in Ruby:

		joebiden = User.new({username: "joebiden", password:"VP4life"})
		joebiden.save
		
These Ruby statements, which use Ruby methods (new and save) and Ruby data types (hashes, strings, variables), translate themselves into the SQL statement:
		
		INSERT INTO users(primary key, username, password) VALUES (2, "joebiden", "VP4life); 

Which inserts the data into the database. A key difference we can see already is that Active Record handles the messy business of managing [primary keys](http://www.google.com) for us, whereas in SQL we need to ensure we're using a unique integer as a primary key. 