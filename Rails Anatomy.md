# Rails Anatomy

#### Layout file: app/views/layouts/application.html.erb (Embedded Ruby)

The layout file is like Python's base.htm: It's the "frame" where new content will fit into. 

As in Python, it would make sense to include head data, header & nav, and footer; essentially only sections of the body will be dynamic. 

		<% ... %>		# Executes code
		<%= ... %> 		# Executes and inserts into template
		
#### Public directory:

Files in the public directory are served directly to the browser (not rendered). The default, static index.html page lives here.

Files in the public directory never hit the main Rails stack; they are delivered directly from the filesystem.

