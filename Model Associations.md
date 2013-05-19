# Rails Model Associations

#### One-to-One Associations:

The difference between `belongs_to` and `has_one` is a semantic one. The model that declares `belongs_to` includes a column containing the foreign key of the other. The model that declares `has_one` has its foreign key referenced. 

`belongs_to`:

	class Orders < ActiveRecord::Base
	  # I reference a customer.
	  belongs_to :customer
	end
	
![Illustration of a belongs_to relationship](http://guides.rubyonrails.org/images/belongs_to.png)

`has_one`
	
	class Supplier < ActiveRecord::Base
	  # One account references me.
	  has_one :account
	end
	
![Illustration of a has_one relationship](http://guides.rubyonrails.org/images/has_one.png)

If you're finding it difficult to recall which is which (they're saying the same thing, right?), remember that _you_ know who your heart _belongs to_, but if you _have one_ secret admirer, only they know about the relationship. Creepy. 

`has_one :through`

Suppliers have one account, and accounts have one account history. By the transitive property, suppliers have one account history. 

![Illustration of a has_one :through relationship](http://guides.rubyonrails.org/images/has_one_through.png)

#### One-to-Many Associations

`has_many`

Following the same `has`-styled relationship of `has_one` and `has_one :through`, the class that declares `has_many` is referenced by many objects that `belong_to` it. 

Justin Bieber `has_many` fans, who `belong_to` him. _They_ keep his foreign key (aka poster on the wall), and he doesn't know they exist.  

	class Customer < ActiveRecord::Base
	  has_many :orders
	end

![Illustration of a has_many relationship](http://guides.rubyonrails.org/images/has_many.png)

`has_many :through`

`has_many :through` can be used to establish shortcuts through _nested relationships_, as with a document that has many paragraphs, which have many sections. A document also has many paragraphs because it has many sections. 

		class Document < ActiveRecord::Base
			has_many :sections
			has_many :paragraphs :through => :sections
		end
		
		class Section < ActiveRecord::Base
			belongs_to :document
			has_many :paragraphs
		end
		
		class Paragraph < ActiveRecord::Base
			belongs_to :section
		end
		
Via this nested association, `has_many :through`, Rails can now make sense of the statement:

		@document.paragraphs
		
`has_many :through` is also useful for describing `has_and_belongs_to_many` "by association" relationships. The Rails documentation provides the example of patients and doctors that each have many appointments and likewise have many of one another (patients or doctors) via appointments, although this example could also be appropriately described by a `has_and_belongs_to_many` relationship.

![Illustration of a has_many :through relationship](http://guides.rubyonrails.org/images/has_many_through.png)

Now Rails recognizes:

		physicians.patients
		
`has_and_belongs_to_many`

A `has_and_belongs_to_many` relationship is common of networks--both real and digital. Doctors have many patients and those patients may have many doctors. Facebook accounts have many friends and belong to many friends lists. 

![Illustration of a has_and_belongs_to_many relationship](http://guides.rubyonrails.org/images/habtm.png)
		
