# Migrations

[Rails](http://www.google.com) migrations allow you to alter your database in a structured and organized way. Instead of altering your database using a [Data Definition Language](http://www.google.com) (the most familiar being [SQL](http://www.google.com)), you'll write Ruby code that [Active Record](https://github.com/brettshollenberger/ruby_wiki/blob/master/Active%20Record.md) will translate into the proper DDL commands. There are a few benefits to handling [schema](https://github.com/brettshollenberger/ruby_wiki/blob/master/Schema.md) changes this way:

* Ruby-fied schema changes via Rails migrations are _database independent_: They aren't written in the DDL of your database, so if you use [SQLite](http://www.google.com) now, and need to transition to [Postgres](http://www.google.com) later, you don't have to change any Ruby code. Active Record will switch to the appropriate DDL for you.
* You don't send your team SQL commands to run: You send them the migrations, which are ordered chronologically. They only have to update to the latest source code and run one command (in the command line, in the [Rails root](https://github.com/brettshollenberger/ruby_wiki/blob/master/Rails%20Root.md)):

		rake db:migrate
		
	This simple command:

	* Keeps track of all migrations (changes to the [database schema](https://github.com/brettshollenberger/ruby_wiki/blob/master/Schema.md)) for the next time you deploy.
	* Keeps everyone's `db/schema.db` up-to-date (file path relative to the Rails root).
* Since migrations are tracked chronologically, when you're ready to update your production code, running `rake db:migrate` will run all the migrations in the same order they were run on the development server: leaving you with an up-to-date database. Try saying that five times fast. 

The only step left for you take care of is to update the [Model class](http://google.com) associated with the changes you made to the schema (making sure you follow the [Migration Naming Convention](https://github.com/brettshollenberger/ruby_wiki/blob/master/Naming%20Migrations.md).

Click here to learn about [writing a Rails migration](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Migration.md).