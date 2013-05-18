# Migration Methods

#### Create A Table:

		create_table :products do |t|
		  t.string :name, :null => false
		  t.integer :name
		end
		
The object yielded to the block in the create table method allows you to create columns. In this case, you can see we've generated to columns. 

The above example is the "sexy" method of column creation, which uses the column type as the method on the column creator object. Below is the older school method that still gets the job done:

		create_table :products do |t|
		  t.column :name, :string, :null => false
		end

The only real difference here is swapping in the column method and adding column type as an argument. Both work. One uses less code. You can tell which one I prefer. 

#### Change A Table

Much like the create_table method, only the object yielded to the block knows more tricks. For example, we can remove the description and name columns, create a part_number column and add an index to it, and rename the column upccode to upc_code.

		change_table :products do |t|
		  t.remove :description, :name
		  t.string :part_number
		  t.index :part_number
		  t.rename :upccode, :upc_code
		end


#### Add/Remove Columns:

		def up
			add_column :table, :column, :type, :options
		end
		
		def down
			remove_column :table, :column
		end
		
Type options: `:string`, `:text`, `:integer`, `:float`, `:decimal`, `:datetime`, `:timestamp`, `:time`, `:date`, `:binary`, and `:boolean`.

Options include `:null` `:default` and `:limit`.

E.g.:

		add column :users, :username, :string, options: { null: false, limit: 30 } # A default value could be added, but it wouldn't make sense to have a default username.
		
#### Add/Remove Index:

Despite the id index that exists by default for each record, you may want to add indices to columns that are frequently searched in large tables to improve search speed. 

		def up
			add_index :table_name, :column_name, :options
		end
		
		def down
			remove_index :table_name, :column => column_name # Removes index by specifying the column name
			
			:name => index_name # Removes index by specifying the index name
		end
			
Options include: :unique, and :order

		:options { unique: true, order: { :name => :desc } }
		
#### Rename Table:

		def up
			rename_table :old_name, :new_name
		end
		
		def down
			rename_table :new_name, :old_name
		end

#### Change Column Type & Options:

		def up
			change_column :table, :column, :new_type, :new_options
		end
		
		def down
			change_column :table, :column, :old_type, :old_options
		end
		


