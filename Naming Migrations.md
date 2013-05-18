# Naming Migrations

* Migrations are named:
		
		YYYYMMDDHHMMSS_migration_name.rb		# UTC timestamp
		
* The latter part of the name (migration_name) should match the name of the [Model class](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Model.md).

		class MigrationName < ActiveRecord::Base