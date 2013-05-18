# Writing a Rails Model

A Rails Model is a Ruby [class](http://google.com) that can add [database records](http://google.com) (think of whole rows in an Excel table), find particular data you're looking for, update that data, or remove data. These common operations are referred to by the acronym [CRUD](http://google.com)--Create, Remove, Update, Destroy. 

There are other ways to perform CRUD operations on databases--the most common  way is to use a [Data Definition Language](http://google.com) like [SQL](http://google.com)--but Rails makes the whole business a lot easier and more extensible for reasons described in the entry on [migrations](https://github.com/brettshollenberger/ruby_wiki/blob/master/Migrations.md). 

Since the model can perform CRUD operations on an _entire_ table in the database, it should be given a description of every column in that table. Again, thinking of Excel, if we had a table for a users in our system, we would want to think of each type of data we'd like to keep track of for individual users. In the following example, I've used just a username and password. (We wouldn't actually want to store unencrypted passwords in the database, but we'll leave that concept out for now). 

The simplest way to write a model class is to let Rails do it for you. Rails contains a model generator, which you can use via your [command line](http://google.com), as long as you're in a Rails app already.

		rails generate model ModelName ColumnOneName:ColumnOneType ColumnTwoName:ColumnTwoType
		
For example: 

		rails generate model User username:string password:string
		
As we'll see in the command line, the model generator generates a number of files for us:

		create    db/migrate/20130518173035_create_users.rb
		create    app/models/user.rb

		create      test/unit/user_test.rb
		create      test/fixtures/users.yml

I've left out a few statements to show only the files that have been generated for us.

1) First is the migration file, which is stored in `db/migrate` like all other migrations files. Migrations describe the changes we'll be making to our database (though haven't actually enacted just yet). In this case, the generated migrations file says: 

		class CreateUsers < ActiveRecord::Migration
		  def change
		    create_table :users do |t|
		      t.string :username
		      t.string :password
		
		      t.timestamps
		    end
		  end
		end
		
Let's break this down:

The first line says: Create a class called "CreateUsers" that [inherits](http://google.com) its functionality from Active Record's Migration class. That line's already a doozy, but here's the breakdown's breakdown: The Migrations class in Active Record contains helper methods that perform CRUD operations, like add_table, add_column, and rename_table. These are the types of things we our class (CreateUsers) to do, so it inherits those methods, giving our class the ability to perform them. 

The second line says, create a [method](http://google.com) called "change." The method name is [convention](http://googe.com) in Rails for [constructive migrations](http://google.com)--aka migrations that _add_ something to a database (in this case we're adding a whole table, but add_column and add_index are also constructive migrations). Since writing a Rails model will always be a constructive migration, we won't explore the other options in this article; check out [Writing a Rails Migration] for details on convention for destructive or change-oriented migrations.

If we had a _destructive_ or _change-oriented_ migration, like drop_table or change_column, we would have wanted to name this method "up" instead of "change." 

		def up
			change_column :users, :username, :email
		end

		def down
  			change_column :users, :email, :username
		end
 


		


