# Annotate Models

The annotate gem creates comments in your model classes to remind you of the structure of that part of your schema. For example:

	user.rb
	# == Schema Information
	#
	# Table name: users
	#
	#  id         :integer          not null, primary key
	#  name       :string(255)
	#  email      :string(255)
	#  created_at :datetime         not null
	#  updated_at :datetime         not null
	#  password   :string(255)
	#
	class User < ActiveRecord::Base
	  attr_accessible :email, :name, :password
	end
	
To create these annotations:

1) In your Gemfile:

	group :development do
		gem 'annotate'
	end

2) In Terminal:

	bundle exec annotate --position before
	
3) Run annotate any time your model changes to keep the annotations up-to-date