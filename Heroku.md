# Heroku

* To deploy to Heroku, include the PostgreSQL gem:

		group :production do
			gem 'pg', '0.12.2'
		end