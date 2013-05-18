# Changing Migrations

If you make a mistake when writing a [migration](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Migration.md) you have a few options:

* If other devs have run the migration, the migration is committed to source control, or (eek) the migration has already been run on production machines: _write a new migration_.
* If none of the above, you can edit the migration by running (in the command line, in the [Rails root](https://github.com/brettshollenberger/ruby_wiki/blob/master/Rails%20Root.md)):
		
		rake db:rollback
