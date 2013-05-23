# Active Record Referential Integrity

> "Garbage in, garbage out."
-Ruby Buddha

Data is the backbone of your application. Allow a user to enter bad data, and you've crippled yourself. 

Data can be validated in a number of locations:

* _On the front-end_ aka the form the user enters the data into. If you've ever entered data on a page that told you that you'd entered incorrect information before you'd even submitted it, you've been validated on the front-end. Front-end validations were traditionally performed via Javascript, and can now also be performed using HTML5. Check here for [Front-End Validations](http://google.com)
* _In the model_. When Rails receives the form from the user, and before it passes it to the database, it can perform its own validations. Validations in the model layer can be very robust. Check here for a list of [Active Record Model Validations](http://google.com)
* _In the controller_. If you're coming from another object-oriented language, you may be tempted to perform validations in the controller. Ruby Buddha also advises to keep controllers skinny. Controller validations just ain't the Rails way.
* _In the database_. Database validations are called _database constraints because they literally control what can and cannot be entered into the database. They should be your last line of defense against bad data. Check here for more on [Database Constraints](http://google.com).

#### The Rails Way of Performing Validations
Active Record (one of two modules that makes up the Rails model layer) claims that validation intelligence belongs in your models, not in the database. As such, features such as triggers or foreign key constraints, which push some of that intelligence back into the database, are not widely used.

Like anything which operates at the application level, these cannot guarantee referential integrity and so some people augment them with foreign key constraints in the database.

Although Active Record does not provide any tools for working directly with such features, the execute method can be used to execute arbitrary SQL. You could also use some plugin like foreigner which adds foreign key support to Active Record (including support for dumping foreign keys in db/schema.rb).

Click here for the Big Bad Super List of [Active Record Model Validations](http://google.com), which will get you started validating the Rails way. 