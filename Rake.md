# Rake Commands

#### To see all the Rake tasks available, run

	$ rake -T
	
#### To see available route mappings, run

	$ rake routes
	
Which returns a list that looks like:

	users GET		/users(.:format)		users#index
    POST   			/users(.:format)		users#create
    new_user GET	/users/new(.:format)	users#new
    
On the left-hand side, we see the request entered in the user's browser (relative to the base URL). In the first example, we have http://0.0.0.0:3000/users, and a [GET request](http://google.com), which will match /users, and map to the index method in the users controller. 