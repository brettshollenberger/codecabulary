# Route Globbing

In certain situations, we may want to create routes that allow for greater flexibility than specific positional parameters like:

	/books/:id
	
Let's say we want to match:

	/books/author/dickens
	
Or

	/books/author/dickens/title/little-dorritt
	
Or

	/books/title/little-dorrit/author/dickens
	
We can accomplish this via route blobbing:

	get 'books/*specs' => 'books#index'
	
Now the Books#index action will have access to a variable number of parameters:

	def index
		@books = Books.where(Hash[*params[:specs].split('/')])
	end