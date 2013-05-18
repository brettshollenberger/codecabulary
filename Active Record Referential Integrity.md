# Active Record Referential Integrity

The Active Record way claims that intelligence belongs in your models, not in the database. As such, features such as triggers or foreign key constraints, which push some of that intelligence back into the database, are not heavily used.

Validations such as validates :foreign_key, :uniqueness => true are one way in which models can enforce data integrity. 

The :dependent option on associations allows models to automatically destroy child objects when the parent is destroyed. 

Like anything which operates at the application level, these cannot guarantee referential integrity and so some people augment them with foreign key constraints in the database.

Although Active Record does not provide any tools for working directly with such features, the execute method can be used to execute arbitrary SQL. You could also use some plugin like foreigner which add foreign key support to Active Record (including support for dumping foreign keys in db/schema.rb).