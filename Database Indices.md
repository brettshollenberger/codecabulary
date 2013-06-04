# Database Indices

Database indices work like book indices: Without them, you'd have to flip through every page to find occurrences of particular topics.

In both cases, indices are only useful if they make it easier to find useful information. You wouldn't have a book indexed with every word in the book, because no one needs to find every occurrence of the definitives and conjunctions. For the same reason, it's important to ask yourself how you'll need to access information on a given table. Will you need to look up users by email? Username? Both?

Answering this question up front will keep your table efficient later. Without the right database indices, you'll end up asking the database to perform a _full-table scan_ each time you use a method like find_by_email. 

If you do need to retroactively add an index to a table column, you can:

1) In Terminal

	rails g migration add_index_to_users_email
	
2) Write the migration:

	def change
		add_index :users, :email, unique: true
	end
	
3) Of course:

	rake db:migrate