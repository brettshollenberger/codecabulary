# Active Record

Apps generally contain three types of things: The stuff the user sees (the view), the saved information (the model), and the logic that delivers the right information to the right user (the controller) (Read more on the [Model-View-Controller Pattern](http://google.com)). 

Active Record is a part of [Rails](http://google.com) that standardizes and simplifies many common tasks involved in working with a part of the model layer--databases. 

In order to use the standardized methods that Active Record provides for working with databases, you'll have to make sure your model classes subclass a class defined in the Active Record library named "Base" (since it's the _base_ for your model). 

By default, Rails will make sure your model classes get the subclassing right if you use the `rails generate model` command to create your model. For example, once you have a [new Rails app](http://google.com), you can enter the command line, [cd](http://www.google.com) into the [Rails root](http://www.google.com), and type:

		rails generate model User username:string password:string
		
In this example we've created a model for a single User, who has a username and a password (which are both [strings](http://www.google.com)). With this one magic command, Rails creates a file named user.rb in the app/models folder of the Rails root (as well as a few other files we'll explore briefly). By default, this is the generated file:

		# The file inherits from ActiveRecord's base class
		class User < ActiveRecord::Base				  
			attr_accessible :username, :password
		end

By [subclassing](http://www.google.com) Base, the User class inherits all the methods defined in the Base class in the Active Record gem. These methods act as a bridge between [instances](http://www.google.com) of the model class and the database table these instances relate to (in this example, the _users_ table--note the pluralization of the given input, User). This bridge isn't quite ready to be crossed yet, but that's the topic of another article: [Setting Up the Rails Model Layer](http://www.google.com). 

#### What is the Base class?
The Base class in Active Record is an [Object-Relational Mapper](http://www.google.com), meaning it [abstracts](http://www.google.com) the details of working with a relational database. Instead of writing [SQL](http://www.google.com) statements (or statements in a similar [Data Definition Language](http://www.google.com), you can write statements in Ruby:

		joebiden = User.new({username: "joebiden", password:"VP4life"})
		joebiden.save
		
These Ruby statements, which use Ruby methods (new and save) and Ruby data types (hashes, strings, variables), translate themselves into the SQL statement:
		
		INSERT INTO users(primary key, username, password) 
		VALUES (2, "joebiden", "VP4life); 

Which inserts the data into the database. A key difference we can see already is that Active Record handles the messy business of managing [primary keys](http://www.google.com) for us, whereas in SQL we need to ensure we're using a unique integer as a primary key. 

Active Record is one of two libraries ([gems](https://github.com/brettshollenberger/ruby_wiki/blob/master/Gems.md)) that ship with Rails that are used to access the [model layer](http://www.google.com) (the other being [Active Model](http://www.google.com)).