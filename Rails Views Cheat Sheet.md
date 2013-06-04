# Rails Views Cheat Sheet

Rails Views files are written in .erb (Embedded Ruby); they _embed_ Ruby evaluations in your HTML. 

#### The erb tag

	<% %>
		
The standard erb tag will evaluate the Ruby inside it, but will not print it to the page. It can be used to render HTML conditionally.

	<% if controller.action_name == "index" %>
		<p>You're on the index page</p>
	<% end %>
		
#### Render erb

	<%= %>
		
The added `=` instructs Ruby to render what it evaluates to the page.

	<%= Time.now %>
		
Will render: 2013-05-30 15:37:43 -0400 on the page. Without the `=` sign, the contents would evaluate but not render on the page. 

#### The `link_to` helper

	<%= link_to "linked_text", "URI", options hash %>
		
The link_to helper is a standard Ruby method (albeit, often written without parentheses), that takes as arguments 1) The anchor text of the link, 2) The URI, and 3) An options hash. Rails helpers often take options hashes in this way in order to give us the flexibility to add arbitrary HTML options without ever leaving Rails. 

	<%= link_to "Sample Link", "http://google.com", id:logo %>
		
Renders the HTML:

	<a href="http://google.com" id="logo">Sample Link</a>
		
In this way we can dynamically create links using erb.

#### The stylesheet_link_tag helper and javascript_include_tag helper

	stylesheet_link_tag "application", media: "all"
		
Again, these helpers are standard Ruby methods that take as arguments 1) the resource path, and 2) an options hash. The `stylesheet_link_tag` helper, for instance, makes it easier to include all the stylesheets required for a site, which in plain HTML need to be added via the <link> tag. 

Further exploring this example, the stylesheet helper will write a link tag for the `application.css` file, which is standard in your Rails app. By default, `application.css` includes the statement `require_tree`, which also asks the `stylesheet_link_tag` helper to write <link> tags for all the other files in the `app/assets/stylesheets` folder. 
