# Rails Model Associations

#### 1-to-1 Associations:

The difference between `belongs_to` and `has_one` is a semantic one. The model that declares `belongs_to` includes a column containing the foreign key of the other. The model that declares `has_one` has its foreign key referenced. 

`belongs_to`:

	class User < ActiveRecord::Base
	  # I reference an account.
	  belongs_to :account
	end
	
![img](http://guides.rubyonrails.org/images/belongs_to.png)

`has_one`
	class Account < ActiveRecord::Base
	  # One user references me.
	  has_one :user
	end
