# Rails Router

The Rails Router has two purposes: 1) to recognize URLs and dispatch them to a [controller's action](google.com), and 2) to generate paths and URLs so you don't need to hardcode them into your programs. 

## URL Mapping:

_Explicit Form_

The mapping can be explicit:

		match "/contact", to: "static_pages#contact"
		
This routes all incoming requests for http://localhost:3000/contact (and eventually yourdomain.com/contact) to the contact action in the static_pages controller like a telephone dispatcher. 

_Implicit Form_

If the URL to match and controller/action combination are the same, you can simply write:

		get "/static_pages/contact"
		
Which will map http://localhost:3000/static_pages/contact to the contact action in the static_pages controller. 

> URLs are matched in the order they are declared. The router will direct the request to the first controller the request matches.

_The Root Route_

To match the root ("/", "http://localhost:3000/," or "yoursite.com"), use:

		root to: "static_pages#index"
		
Which maps the root to the index action in the static_pages controller.

_Resourceful Routes_

| HTTP Verb | CRUD     | Path            | Action | Used To                                                    |
| --------------  |:------------:|:---------------:|:--------:| ------------------------------------------------------:|
| GET            | Retrieve | /users         | index  | Display a list of all users                            |
| GET            | Retrieve | /users/new | new     | Return an HTML form for creating a user |
| POST          | Create   | /users         | create | Create a user                                            |

| HTTP Verb	 | CRUD	|Path     	        | Action  	| Used To |
| ------------------ |:-----------:|-------------------:|------------:|-------------:|
| GET           	| Retrieve 	| /users		| index	| Display a list of all users |
| GET      		| Retrieve	| /users/show	| new	| Return an HTML form for creating a user |
| POST  		| Create   	| /users		| create	| Create a user |
| GET 		| Retrieve 	| /users/:id		| show	| Display a specific user |
| GET  		| Retrieve 	| /users/:id/edit	| edit		| Return an HTML form for editing a user |
| PUT  		| Update   	| /users/:id		| update	| Update a specific user |
| DELETE  	| Destroy  	| /users/:id		| destroy	| Delete a specific user |


##Path and URL Generation

_Named Routes_

Route declarations automatically create "named routes:" variables that you can refer to in the rest of your Rails program. For instance:

		match "/about", to: "static_pages#about"
		
Automatically creates the _named route_ "about_path."

	match "/about" 						=> about_path
	match "/about_us" 					=> about_us_path
	match "/about_us_were_very_cool" 	=> about_us_were_very_cool_path

_Even though named paths are automatically created, they should usually be explicitly defined._

		match "/about_us", to: "static_pages#about", as: "about"
		
The `as:` option defines the named route. Although the dev in this example has allowed "about_us" to be matched to the about action, the named route will stay the same.

Defining the `as` option explicitly makes your code less brittle: if you use the named route defined via an `as` option throughout your tests, you won't break anything by changing the URL to that maps to the controller. 
