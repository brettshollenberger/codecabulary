# Database Locking

When multiple users may update a single resource at a given time in an application, we should implement some sort of database locking mechanism to avoid race conditions and possible errors where one user's changes get thrown away. 

In many applications, individual resources ideologically belong to individual users, and in these applications we don't need to worry about database locking. In Rails, database locking is not implemented by default. But in applications where users may update the same resource, we have a couple of options to handle concurrency.

## Optimistic Locking

Optimistic Locking is generally implemented in applications where race conditions are considered to be unlikely. As a result, optimistic locking is easy to implement, but not a terrific user experience. To implement optimistic locking in Rails, simply add a `:lock_version` column to a database record, which records the version of the current record:

	def change
		add_column :timesheets, :lock_version, :integer, default: 0
	end
	
By default, Rails will handle incrementing the version on save. To use another column as the `lock_version`, we can either set a default in `application.rb` or on a per-model basis:

	# application.rb
	config.active_record.locking_column = :alternate_locking_column
	
	# timesheet.rb
	class Timesheet < ActiveRecordBase
		self.locking_column = :alternate_locking_column
	end
	
We can show via unit tests that the optimistic locking works:

	describe Timesheet
		before(:each)
			@timesheet   = Timesheet.create()
			@timesheet2 = Timesheet.find(@timesheet.id)
		end
		
		it "locks optimistically" do
			@timesheet.rate = 175
			@timesheet2.rate = 200
			
			expect(@timesheet.save).to be_true
			expect { @timesheet2.save }.to raise_error(ActiveRecord::StaleObjectError)
		end
	end

We notice that saving `@timesheet2` raises an `ActiveRecord::StaleObjectError`, allowing us to handle it in our controller:

	def update
		@timesheet = Timesheet.find(params[:id])
		@timesheet.update(timesheet_params)
		
		rescue ActiveRecord::StaleObjectError
			flash[:error] = "The timesheet was updated while you were editing it"
			redirect_to [:edit, @timesheet]
		end
	end

The benefits of optimistic locking are that it is easy to implement and requires no special database features to do so. It's baked directly into ActiveRecord, making our lives easy. However, it doesn't make for a great user experience, since the user only sees the error once they've updated unsuccessfully. It would usually make sense to also provide the user--somehow--with the changes they previously tried to submit. Optimistic locking is not recommended when race conditions are considered to be likely in an application.

## Pessimistic Locking

