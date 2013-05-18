# Rails Model Associations

#### 1-to-1 Associations:

	class User < ActiveRecord::Base
	  # I reference an account.
	  belongs_to :account
	end
	
	class Account < ActiveRecord::Base
	  # One user references me.
	  has_one :user
	end
	
![img](http://guides.rubyonrails.org/images/belongs_to.png)