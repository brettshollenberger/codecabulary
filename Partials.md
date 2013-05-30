# Partials

Partials are reusable snippets of Rails view code, meaning you can use them to DRY it out.

#### Install the Rails Partials Package for Sublime Text 2

1) Press command shift P

2) Select Install Package

3) Select Rails Partials

#### Create a Partial

1) Highlight a snippet you want to reuse

2) Press alt P

3) Name the partial

4) Sublime will add the leading underscore and filetype (.html.erb) that denotes a partial

5) If you create a partial in the layout file, it _must_ go in app/views/application. If the folder doesn't exist, you'll likely have to move it yourself.

6) Render the partial (Sublime will also do this for you):

		<%= render "partial_name" %>

#### Changing Loops to Partials

1) Take your standard each loop:

		@events.each do |event|
			<td><%= event.name %></td>
			<td><%= event.location %></td>
		end

2) Remove the do/end lines:

		<td><%= event.name %></td>
		<td><%= event.location %></td>
		
3) Make it a partial (Alt P, name it). In this case, I named it "event."

4) Change the render line to a hash including the collection option:

		<%= render partial: "event", collection: @events %>