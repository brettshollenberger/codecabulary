# Writing a Rails Migration



[Rails migrations](https://github.com/brettshollenberger/ruby_wiki/blob/master/Migrations.md) are just Ruby code. In your [Rails app](http://google.com), your migrations are stored in `db/migrate` (relative to the [Rails root](https://github.com/brettshollenberger/ruby_wiki/blob/master/Rails%20Root.md)). 

A migration, by definition, is a _change_ to the [database schema](https://github.com/brettshollenberger/ruby_wiki/blob/master/Schema.md), so if you haven't yet created a table in your database or generated a [Model class](http://google.com) to talk to that database table, you'll want to start by [Writing a Rails Model](http://google.com). 



		
For example, to generate a migration for  

To write one, start by defining a class whose name describes the change it affects on the database:

		class CreateProducts < ActiveRecord::Migration
		
If we had a _destructive_ or _change-oriented_ migration, like drop_table or change_column, we would have wanted to name this method "up" instead of "change." 

		def up
			change_column :users, :username, :email
		end

		def down
  			change_column :users, :email, :username
		end
