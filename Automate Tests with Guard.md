# Automate Tests with Guard

#### Installing Guard 

1) Add guard-rspec to the Gemfile

		group :development, :test do
			'guard-rspec'
		end
		
2) Add other :test group gems

		group :test do
			gem 'rspec-rails'
			gem 'capybara'
			gem 'rb-fsevent'
			gem 'growl'
		end

3) `bundle`

4) `guard init`

5) Edit the Guardfile to add:

		require 'active_support/core_ext'
		
		guard 'spec, :all_after_pass => false do