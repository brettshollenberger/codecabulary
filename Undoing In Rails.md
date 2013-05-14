# Undoing in Rails

Undoing `rails generate controller`

		rails generate controller FooBars baz quux
		
		rails destroy  controller FooBars baz quux
		
Undoing `rails generate model`

		rails generate model Foo bar:string baz:integer
		
		rails destroy model Foo
		
Undoing `db:migrate`

		rake db:migrate
		
		rake db:rollback
		
To go back to a particular version, use:

		rake db:migrate VERSION=0