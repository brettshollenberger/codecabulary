# Named Routes

Named routes help us to define RESTful routes, and make it easy for us as programmers to create links.

When we name a route, a new method gets defined:

	get 'books/:id' => 'books#show', as: 'book'
	
	app.book_path(1)
	>> '/books/1'
	
## Syntactic Sugar

We can and should also pass in objects to our helper to create the path:

	<%= link_to @book.title, book_path(@book) %>
	
This will call `to_param` on the item we pass in, and pass off the object's `id`.

If we want to instead use a title in our route, we can override the the `to_param` method in our Book model:

	# book.rb
	
	def to_param
		title.parameterize
	end
	
	@book.to_param
	// "little-dorrit"
	
Of course, now we must make precautions to dig out the Book again. We could find it using `titleize`, perhaps:

	def show
		@book = Book.where(title: params[:title].titleize).first
	end
	
Or if it's really munged, we could add a new field:

	def show
		@book = Book.where(munged_title: params[:title])
	end