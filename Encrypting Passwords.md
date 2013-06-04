# Encrypting Passwords

Often, we'll want to protect our users by requiring:

1) A password to match a "password confirmation" entry (make the user enter the password twice to minimize typos)

2) Encrypting their passwords in our database 

We'll then authenticate users via an _authenticate_ method, which will:

* Encrypt a submitted password

* Compare it to the stored encrypted password

* And return the user object or false

Here's how we'll get it done:

1) Add bcrypt-ruby to your Gemfile

2) Add a password_digest column to your user model. 

> Note: The name password_digest is essential for the bcrypt-ruby magic to work correctly. The "digest" portion of the name refers to its cryptographic origins--this is the column that will store our encrypted hash.

3) `bundle`

4) `rake db:migrate`

5) Allow :password and :password_confirmation to be mass assigned; we'll need to check them on the model although we won't save them in the database.

	model User < ActiveRecord::Base
		attr_accessible :email, :password, :password_confirmation
	...
	
You're ready to go! Check out our authenticate method:

	user = user.find_by_email("brett.shollenberger@gmail.com") # We need a user to authenticate first
	user.authenticate("foobar")
	#<User id: 1, email: "brett.shollenberger@gmail.com", password_digest: "$2a$10$5E3iP2W.lN4vo542CSvPlOlQg19j6RjyMJyk3CV4tDax...", created_at: "2013-06-04 12:58:47", updated_at: "2013-06-04 12:58:47">
