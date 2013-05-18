# Database Commands

		rake db:migrate VERSION=20080906120000		# Version arg optional, and runs all migrations up until the specified version
		
		rake db:rollback STEP=3		# Step arg optional, and specifies the number of migrations to rollback
		
The `rake db:reset` task will drop the database, recreate it and load the current schema into it. This is clearly different from rolling back and running migrations again. All your data will be dropped.

		rake db:reset
