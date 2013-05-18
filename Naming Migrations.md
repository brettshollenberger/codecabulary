# Migration Naming Convention

* Migrations should be named following this convention::
		
		YYYYMMDDHHMMSS_migration_name.rb		# UTC timestamp
		
* The latter part of the name (migration_name) should match the name of the [Model class](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Model.md).

		class MigrationName < ActiveRecord::Base
		
* If you generate these files using the built-in [Rails migration generator](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Migration.md), it will follow these conventions for you.  