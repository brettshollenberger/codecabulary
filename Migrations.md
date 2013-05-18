# Migrations

Rails migrations allow you to alter your database in a structured and organized way. Gone are the days of sending the rest of the team the specific [SQL](http://www.google.com) commands you ran to ensure their databases match yours; all your team has to do to make their databases match is to update to the latest source code with [Git](http://www.google.com) and (in the command line, in the [Rails root](https://github.com/brettshollenberger/ruby_wiki/blob/master/Rails%20Root.md)) run:

		rake db:migrate
		
This simple command:

* Keeps track of all migrations (changes to the [database schema](https://github.com/brettshollenberger/ruby_wiki/blob/master/Schema.md)) for the next time you deploy.
* Keeps everyone's `db/schema.db` up-to-date (file path relative to the Rails root).

The only step left for you is to update the associated [Model class](http://google.com) to reflect the changes you made. 