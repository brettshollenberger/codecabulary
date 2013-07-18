# Namespaced Routes

You want your users to have a products page, and your admin to have a special products page; you need an admin backend, and a good way to separate these concerns is via a namespace.

If you know about modules, you already know about namespaces. To set up your namespaced routes:

config/routes.rb:

	Routes::Application.routes.draw do
	  namespace :admin do
	    resources :products
	  end
	
	  resources :products
	
	  root to: "products#index"
	end

Now `rake routes` produces:

	    admin_products GET    /admin/products(.:format)          admin/products#index
               		   POST   /admin/products(.:format)          admin/products#create
	 new_admin_product GET    /admin/products/new(.:format)      admin/products#new
	edit_admin_product GET    /admin/products/:id/edit(.:format) admin/products#edit
	     admin_product GET    /admin/products/:id(.:format)      admin/products#show
	                   PUT    /admin/products/:id(.:format)      admin/products#update
	                   DELETE /admin/products/:id(.:format)      admin/products#destroy
	          products GET    /products(.:format)                products#index
	                   POST   /products(.:format)                products#create
	       new_product GET    /products/new(.:format)            products#new
	      edit_product GET    /products/:id/edit(.:format)       products#edit
	           product GET    /products/:id(.:format)            products#show
	                   PUT    /products/:id(.:format)            products#update
	                   DELETE /products/:id(.:format)            products#destroy
	              root        /                                  products#index
	              
In your controllers folder, add an admin folder (for the admin namespace), and inside add the products_controller. Outside the admin namespace, you'll add another products_controller.

Inside the admin products_controller, establish the namespace:

	module Admin
		class ProductsController < ApplicationController
		
		Your controller actions ...
		
		end
	end
	
And in the views, you can establish the same schema, with a products folder under the top views folder, and another products folder under an admin folder. Each of those folders can have the standard `index.html.erb`, `show.html.erb`, etc. without bumping into one another. 