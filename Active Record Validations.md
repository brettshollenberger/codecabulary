# Active Record Validations

The basic structure for Active Record validations is to use the `validates` keyword, followed by a comma-separated list of the `fieldName`s to validate, followed by a comma-separated list of the validations to perform and various options for the validation. Or, in code:

		validates :fieldName, :validation => value, :option => value
		
Rails developers most often place their validations at the top of their models (directly under the class declaration), and before [associations](https://github.com/brettshollenberger/ruby_wiki/blob/master/Model%20Associations.md) and the [`attr_accessible` statement](http://google.com). For example, a standard model class will look like:

		class Song < ActiveRecord::Base
			validates :title, :presence => true	# Validations
			:belongs_to :album					# Associations
			attr_accessible :title, :album_id	# Attr_accessible
			
#### The Big Bad List of Rails Validations

This list, and additional information, is available via the [Rails Guides](http://guides.rubyonrails.org/active_record_validations_callbacks.html). Here, we've included additional clarification where necessary. 

`validates :acceptance`

Validates that a checkbox on the user interface was checked when a form was submitted. This is typically used when the user needs to agree to your application’s terms of service, confirm reading some text, or any similar concept. This validation is very specific to web applications and this ‘acceptance’ does not need to be recorded anywhere in your database (if you don’t have a field for it, the helper will just create a virtual attribute).

		class Person < ActiveRecord::Base
		  validates :terms_of_service, :acceptance => true
		end
		
`validates associated`

You should use this helper when your model has associations with other models and they also need to be validated. When you try to save your object, valid? will be called upon each one of the associated objects.

		class Library < ActiveRecord::Base
		  has_many :books
		  validates_associated :books
		end

`validates confirmation`

You should use this helper when you have two text fields that should receive exactly the same content. For example, you may want to confirm an email address or a password. This validation creates a virtual attribute whose name is the name of the field that has to be confirmed with “_confirmation” appended.

		class Person < ActiveRecord::Base
		  validates :email, :confirmation => true
		end
In your view template you could use something like

		<%= text_field :person, :email %>
		<%= text_field :person, :email_confirmation %>
		
This check is performed only if email_confirmation is not nil. To require confirmation, make sure to add a presence check for the confirmation attribute (we’ll take a look at presence later on this guide):
		
		class Person < ActiveRecord::Base
		  validates :email, :confirmation => true
		  validates :email_confirmation, :presence => true
		end
		
The default error message for this helper is “doesn’t match confirmation”.

`validates exclusion`

This helper validates that the attributes’ values are not included in a given set. In fact, this set can be any enumerable object.

	class Account < ActiveRecord::Base
	  validates :subdomain, :exclusion => { :in => %w(www us ca jp),
	    :message => "Subdomain %{value} is reserved." }
	end

The exclusion helper has an option :in that receives the set of values that will not be accepted for the validated attributes. The :in option has an alias called :within that you can use for the same purpose, if you’d like to. This example uses the :message option to show how you can include the attribute’s value.

The default error message is “is reserved”.

`validates format`

This helper validates the attributes’ values by testing whether they match a given regular expression, which is specified using the :with option.

		class Product < ActiveRecord::Base
		  validates :legacy_code, :format => { :with => /\A[a-zA-Z]+\z/,
		    :message => "Only letters allowed" }
		end

The default error message is “is invalid”.

`validates inclusion`

This helper validates that the attributes’ values are included in a given set. In fact, this set can be any enumerable object.
	
		class Coffee < ActiveRecord::Base
		  validates :size, :inclusion => { :in => %w(small medium large),
		    :message => "%{value} is not a valid size" }
		end
		
The inclusion helper has an option :in that receives the set of values that will be accepted. The :in option has an alias called :within that you can use for the same purpose, if you’d like to. The previous example uses the :message option to show how you can include the attribute’s value.

The default error message for this helper is “is not included in the list”.

`validates length`

This helper validates the length of the attributes’ values. It provides a variety of options, so you can specify length constraints in different ways:

		class Person < ActiveRecord::Base
		  validates :name, :length => { :minimum => 2 }
		  validates :bio, :length => { :maximum => 500 }
		  validates :password, :length => { :in => 6..20 }
		  validates :registration_number, :length => { :is => 6 }
		end

The possible length constraint options are:

* `:minimum` – The attribute cannot have less than the specified length.
* `:maximum` – The attribute cannot have more than the specified length.
* `:in` (or `:within`) – The attribute length must be included in a given interval. The value for this option must be a range.
* `:is` – The attribute length must be equal to the given value.
	
The default error messages depend on the type of length validation being performed. You can personalize these messages using the :wrong_length, :too_long, and :too_short options and %{count} as a placeholder for the number corresponding to the length constraint being used. You can still use the :message option to specify an error message.

		class Person < ActiveRecord::Base
		  validates :bio, :length => { :maximum => 1000,
		    :too_long => "%{count} characters is the maximum allowed" }
		end

This helper counts characters by default, but you can split the value in a different way using the `:tokenizer` option:

		class Essay < ActiveRecord::Base
		  validates :content, :length => {
		    :minimum   => 300,
		    :maximum   => 400,
		    :tokenizer => lambda { |str| str.scan(/\w+/) },
		    :too_short => "must have at least %{count} words",
		    :too_long  => "must have at most %{count} words"
		  }
		end
		
Note that the default error messages are plural (e.g., “is too short (minimum is %{count} characters)”). For this reason, when :minimum is 1 you should provide a personalized message or use validates_presence_of instead. When :in or :within have a lower limit of 1, you should either provide a personalized message or call presence prior to length.
The size helper is an alias for length.

`validates numericality`

This helper validates that your attributes have only numeric values. By default, it will match an optional sign followed by an integral or floating point number. To specify that only integral numbers are allowed set :only_integer to true.
If you set :only_integer to true, then it will use the
/\A[+-]?\d+\Z/
regular expression to validate the attribute’s value. Otherwise, it will try to convert the value to a number using Float.
Note that the regular expression above allows a trailing newline character.

		class Player < ActiveRecord::Base
		  validates :points, :numericality => true
		  validates :games_played, :numericality => { :only_integer => true }
		end

Besides :only_integer, this helper also accepts the following options to add constraints to acceptable values:

* `:greater_than` – Specifies the value must be greater than the supplied value. The default error message for this option is “must be greater than %{count}”.
* `:greater_than_or_equal_to` – Specifies the value must be greater than or equal to the supplied value. The default error message for this option is “must be greater than or equal to %{count}”.
* `:equal_to` – Specifies the value must be equal to the supplied value. The default error message for this option is “must be equal to %{count}”.
* `:less_than` – Specifies the value must be less than the supplied value. The default error message for this option is “must be less than %{count}”.
* `:less_than_or_equal_to` – Specifies the value must be less than or equal the supplied value. The default error message for this option is “must be less than or equal to %{count}”.
* `:odd` – Specifies the value must be an odd number if set to true. The default error message for this option is “must be odd”.
* `:even` – Specifies the value must be an even number if set to true. The default error message for this option is “must be even”.
	
The default error message is “is not a number”.

`validates presence`

This helper validates that the specified attributes are not empty. It uses the blank? method to check if the value is either nil or a blank string, that is, a string that is either empty or consists of whitespace.

		class Person < ActiveRecord::Base
		  validates :name, :login, :email, :presence => true
		end

If you want to be sure that an association is present, you’ll need to test whether the foreign key used to map the association is present, and not the associated object itself.

		class LineItem < ActiveRecord::Base
		  belongs_to :order
		  validates :order_id, :presence => true
		end
		
Since false.blank? is true, if you want to validate the presence of a boolean field you should use validates :field_name, :inclusion => { :in => [true, false] }.

The default error message is “can’t be empty”.

`validantes uniqueness`

This helper validates that the attribute’s value is unique right before the object gets saved. It does not create a uniqueness constraint in the database, so it may happen that two different database connections create two records with the same value for a column that you intend to be unique. To avoid that, you must create a unique index in your database.

		class Account < ActiveRecord::Base
		  validates :email, :uniqueness => true
		end

The validation happens by performing an SQL query into the model’s table, searching for an existing record with the same value in that attribute.

There is a :scope option that you can use to specify other attributes that are used to limit the uniqueness check:.

		class Holiday < ActiveRecord::Base
		  validates :name, :uniqueness => { :scope => :year,
		    :message => "should happen once per year" }
		end
		
There is also a :case_sensitive option that you can use to define whether the uniqueness constraint will be case sensitive or not. This option defaults to true.

		class Person < ActiveRecord::Base
		  validates :name, :uniqueness => { :case_sensitive => false }
		end

The `:dependent` option on associations allows models to automatically destroy child objects when the parent is destroyed. 
