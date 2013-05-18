# Migration Special Helpers

Some functionality is so common that Rails provides the functionality out of the box. 

#### Add a created_at and updated_at column to a table:

		create_table :products do |t|
		  t.timestamps
		end
		
Timestamps is added by default when generating a [Rails migration](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Migration.md) (it assumes you want them). 

Similarly:

		change_table :products do |t|
		  t.timestamps
		end

Adds those columns to an existing table. 

#### Add a references or belongs_to column

		create_table :products do |t|
		  t.references :category
		end
		
To add the reference, use the model name, not the column name. Active Record will add the _id for you.

>The references helper does not actually create foreign key constraints for you. You will need to use execute or a plugin that adds foreign key support.

>-Rails Guide

