# Database Sandboxing

When learning [Active Record](https://github.com/brettshollenberger/ruby_wiki/blob/master/Active%20Record.md), it can be useful to play around in the [Rails console](google.com) without actually affecting your database. 

To do this, you can run:

	rails console --sandbox
	
Which will automatically [rollback](https://github.com/brettshollenberger/ruby_wiki/blob/master/Undoing%20In%20Rails.md) the changes made when you exit the console. 