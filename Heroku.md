# Heroku

* To deploy to Heroku, include the PostgreSQL gem:

		group :production do
			gem 'pg', '0.12.2'
		end
		
* Avoid use of different databases in development and production (use PostgreSQL on your local machine).
* Install notes on PostgreSQL are included in Section 3.5 of the Hartl tutorial

#### Deploy

		heroku create --stack cedar
		git push heroku master
		
#### Pushing

		git push heroku
		
#### Checking Logs

		heroku logs