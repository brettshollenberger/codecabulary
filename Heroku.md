# Heroku

* To deploy to Heroku, include the PostgreSQL gem:

		group :production do
			gem 'pg', '0.12.2'
		end
		
* Avoid use of different databases in development and production (use PostgreSQL on your local machine).
	* Follow the instructions on the [Heroku help page](https://devcenter.heroku.com/articles/heroku-postgresql#local-setup)
	* As a Mac user, I really only care about me: [Download Postgres.app](http://postgresapp.com). Windows/Linux users should figure out their own ish. 
* Install notes on PostgreSQL are included in Section 3.5 of the Hartl tutorial

#### Deploy

		heroku create --stack cedar
		git push heroku master
		
#### Pushing

		git push heroku
		
#### Checking Logs

		heroku logs