# Controllers

Controllers are a way to bundle Rails _actions_ related by a common purpose. 

#### Ex: Creating a controller called StaticPages with (optional) actions for a Home page and a Help page:

		rails generate StaticPages home help --optional-flags
		
`rails generate controller` automatically updates the Rails router. 
		
#### Undoing the same action:

		rails destroy StaticPages home help
		
`rails destroy` removes not only the controller, but all other files created with the `rails generate` command, and undoes any automatic changes made to the routes.rb file.

#### Rails controllers inherit from the Rails class ApplicationController, giving it a large amount of Rails-specific functionality.

		class StaticPagesController < ApplicationController
		
#### Actions are defined using the def keyword

		def home
		end
		
		def help
		end
		
#### To the browser, raw HTML is indistinguishable from HTML delivered via the controller/action method: All the browser ever sees is HTML.


