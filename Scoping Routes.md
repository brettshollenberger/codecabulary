# Scoping Routes

Rails provides a variety of ways to bundle together related routing rules:

## Controller

	scope :controller => :books do
		get "books" => :index, as: :books
		get "book/:id" => :show, as: :book
	end
	
	controller :books do
		get "books" => "index", as: :books
	end
	
## Path

	scope :path => "/books" do
		get "/" => :index, as: :books
		get "/:id" => :show, as: :book
	end
	
## Name Prefix

	scope :auctions, as: "admin" do
		get "new" => :new, as: :new_auction
	end
	
	>> admin_new_auction_path
	
## Namespace

Sets up a module, name prefix, and path prefix in one declaration:

	namespace :api do
		namespace :v1 do
			controller: :books do
				get "books" => :index, as: :books
				get "book/:id" => :show, as: :book
			end
		end
	end
	
	>> api_v1_books_path(:json)
	>> /api/v1/books.json
	
	>> api_v1_book_path(@book, :json)
	>> /api/v1/books/little-dorrit.json
	
