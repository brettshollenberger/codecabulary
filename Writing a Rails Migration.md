# Writing a Rails Migration



[Rails migrations](https://github.com/brettshollenberger/ruby_wiki/blob/master/Migrations.md) are just Ruby code. In your [Rails app](http://google.com), your migrations are stored in `db/migrate` (relative to the [Rails root](https://github.com/brettshollenberger/ruby_wiki/blob/master/Rails%20Root.md)). 

A migration, by definition, is a _change_ to the [database schema](https://github.com/brettshollenberger/ruby_wiki/blob/master/Schema.md), but the first change to make is to generate a table if you haven't already, which you can review in more detail at [Writing a Rails Model](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Model.md). 

#### 1) Generate the Migration Files

For general changes, the easiest way about it is to have Rails generate the migration files you need for you. In your command line, in the [Rails root](https://github.com/brettshollenberger/ruby_wiki/blob/master/Rails%20Root.md), type:

		rails g migration migration_description
		
For example:

		rails g migration change_username_field_to_email

Like generating a model, Rails will generate a few files for you when generating a migration. The first is the migration file itself, located in `db/migrate`, which in this case would be named `YYYYMMDDHHMMSS_change_username_field_to_email`. Check here for more info on [Migration Naming Conventions](https://github.com/brettshollenberger/ruby_wiki/blob/master/Naming%20Migrations.md).

ProTip: If the migration name you write in this step is of the form “AddXXXToYYY” or “RemoveXXXFromYYY” and is followed by a list of column names and types then a migration containing the appropriate add_column and remove_column statements will be created for you (otherwise the next step in the process). Bada-bing.

Example:

		rails generate migration AddPartNumberToProducts part_number:string

#### 2) Write the Up & Down Methods or Change Method in the Migration file

The migration file will already be written for us, and will start with this line:

		class ClassName < ActiveRecord::Migration
		
In our example:

		class ChangeUsernameFieldToEmail < ActiveRecord::Migration
		
For more info on this line, check out the section on [Writing a Rails Model](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Model.md). 

If we were writing a [constructive migration](http://google.com) like adding a column or a table, we'd want to name the method in the next line `change`. Since instead we're changing a column, we'll need to create two methods: `up` and `down` (where up is the change we want to make, and down is its inverse--a fallback in case we want to undo our changes). Check here for more details on [Changing Migrations](https://github.com/brettshollenberger/ruby_wiki/blob/master/Changing%20Migrations.md).

		def up
			change_column :users, :username, :email
		end

		def down
  			change_column :users, :email, :username
		end
		
How did we know what the names of these methods were? Simple, check [Migration Methods](https://github.com/brettshollenberger/ruby_wiki/blob/master/Migration%20Methods.md) for a complete list of methods we can use to change our database schema.

How did we know whether to use `change` or `up`/`down`? `up`/`down` is going to be the safest bet in all cases (today), but there's also a list of the methods `change` supports:

	•	add_column
	•	add_index
	•	add_timestamps
	•	create_table
	•	remove_timestamps
	•	rename_column
	•	rename_index
	•	rename_table

#### 3) Run the Migration File

The migration is almost complete. Back in the command line, in the Rails root, we'll run:

		rake db:migrate
		
Which will run the migration file, make the changes to our database, and update our schema. 

#### 4) Change the Model Class

The last change is to update the Model class to accurately reflect the change we made. The model file is located in `app/models/modelName.rb`. Again, we changed the name of username to email, so in the class, we'll change the name too:

		class User < ActiveRecord::Base
		  attr_accessible :email, :password
		end
		
That's it! We've successfully changed our database, and the Model class that we use to speak to it. Sweet!

