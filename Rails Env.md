# Rails Env

Rails applications are preconfigured with 3 standard environments, which can be used to specify a large number of environment-specific variables that need to change between the test, development, and production environments. We often find ourselves changing configuration between these environments for things like mail clients, file uploads, eager loading, and any number of other specifics.

Way back in Rails 2, the variable declaring the current environment was specified using the environment variable `ENV["RAILS_ENV"]` . Since Rails 3, this same variable now hangs off of the `Rails` object itself, using `Rails.env`.

The new `Rails.env` variable inherits from `ActiveSupport::StringInquirer`, which allows it to respond to any arbitrary methods inquiring into its value, e.g.:

	Rails.env.production?
	> true
	
	Rails.env.whatever?
	> false
	
The inquiry syntax will work with any prefix, and will only return true if the value of `Rails.env` matches it exactly.