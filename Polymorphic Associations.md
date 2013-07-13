# Polymorphic Associations

Polymorphic associations add flexibility to the `has_many` / `belongs_to` relationship. On Facebook, you can `like` status updates, comments, photos, and videos, and using traditional `has_many` / `belongs_to` relationships for likes, you'd have to generate four models: `likes_status_updates`, `comments_likes`, `likes_photos`, and `likes_videos`. You'd have four controllers and different logic in your various views, and a lot of code that seems like it ought to be DRYed up.

Enter polymorphic associations, which allow you to describe a `like` as `belonging_to` anything `likeable`. One model, one controller, and reusable helpers. Let's see how it works:

1) Set up your `like` model. Since we're replacing a column name like `photo_id` on the `like` model, we'll need to add two columns: `likeable_id` (the foreign key) and `likeable_type` (the foreign model name), which will help Rails to derive the exact object the like belongs to. 

	rails g model like likeable_id:integer likeable_type:string user_id:integer
	
In this example, I've also added a `user_id`, since we'll only want a user to be able to like any one thing _once_. In the generated migration, we'll want to add `null: false` constraints on each of these columns, as we'll need them all to validate. 

2) In the like model, we'll want to make a few changes:

* `attr_accessible` should be shifted to include `user` and `likeable`, instead of the ids of each. 
* We should validate presence of both, as we won't want a like to be legitimized if a user doesn't exist, or whatever they like doesn't exist.
* The like `belongs_to` both a `likeable` and a `user`. In the case of the `likeable`, we'll need to pass in the `:polymorphic => :true` option to declare the polymorphism.
* Finally, to allow a single user to like a single thing only once, we'll validate uniqueness of the user_id, within the scope of the likeable (defined by the 	`likeable_id` and `likeable_type`). In this slightly more complex take on the uniqueness/scoped validation, we'll need to use an array to pass multiple arguments to scope to.


		class Like < ActiveRecord::Base
	  		attr_accessible :user, :likeable
	
		  	validates :user, :likable,  {
		    	presence: true
		  	}
		
		 	 belongs_to :likeable, :polymorphic => true
		
		  	belongs_to :user
		
		  	validates_uniqueness_of :user_id, :scope => [:likeable_id, :likeable_type]
	
		end
		
3) In our router, we'll need to nest the `likes` resource under anything that will be likeable in order for us to create those paths:

	resources :comments do
		resources :likes
	end
	
	resources :status_updates do
		resources :likes
	end
	
Run `rake routes` to check out the routes created by these nested resource statements. They'll take the form of `top_level_resource/:top_level_id/nested_resource/nested_id`
	
3.1) If you have nested resources, you can handle them in a similar fashion, although you should never nest resources deeper than two levels (as in this example): 

	resources :status_updates do
		resources :likes
		resources :comments do
			resources :likes
		end
	end
	
4) In your likes controller, you'll need to create a method to derive the likeable resource. There are a few options:

`The Ryan Bates`:

	private

	def load_commentable
		resource, id = request.path.split('/')[1, 2]
		@commentable = resource.singularize.classify.constantize.find(id)
	end
	
The Ryan Bates method relies on standard, RESTful Rails URL patterns, and would break without them.

`The Single Resource`: 

private

	def find_likeable
	  params.each do |name, value|
	    if name =~ /(.+)_id$/
	      return $1.classify.constantize.find(value)
	    end
	  end
	  nil
	end
	
An example controller action for either `The Ryan Bates` or `The Single Resource` would look like:

	def create
		@likeable = find_likeable
	    	@user = current_user
	    	@like = @user.likes.build(likeable: @likeable)
	    	
	    	if @like.save
	    		redirect_to @likeable, notice: "Liked!"
	    	else
	    		redirect_to @likeable, notice: "Not liked!"
	    	end
	end
	
`The Nested Resource`: 

	private

	def find_likeable
	    likeable = []
	    params.each do |name, value|
	      if name =~ /(.+)_id$/
	        likeable.push($1.classify.constantize.find(value))
	      end
	    end
	    return likeable[0], likeable[1] if likeable.length > 1
	    return likeable[0], nil if likeable.length == 1
	    nil
	end

In the event we have to like a nested resource, as we setup a potential use case for in the routing example, we'll need to pass both the parent and child resource in our link tags, and so we'll have to handle for both in our `find_likeable` method.

And example controller for `The Nested Resource` would look like:

	def create
	    @likeable_parent, @likeable_child = find_likeable
	    @user = current_user
	
	    if @likeable_child == nil
	      @like = @user.likes.build(likeable: @likeable_parent)
	    else
	      @like = @user.likes.build(likeable: @likeable_child)
	    end
	    
	    if @like.save
	    	redirect_to @likeable_parent, notice: "Liked!"
	    else
	    	redirect_to @likeable_parent, notice: "Not liked!"
	    end
	end
	
5) In your views, you'll just add a reference to the create action for your likes nested beneath the proper top-level resource. A given page may have many likeable entities--comments, status updates, photos, etc--so the top-level resource will be the differentiator:

	<%= link_to "Like", new_like_path(@comment, @like) %>

