# Indexing AJAX Applications

Pages that generate content and metadata asynchronously cannot be crawled by Facebook or Google, making all your SEO people's hard work for naught. Their crawlers index your page without executing any Javascript, meaning that they'll see your templating engine's inner workings instead of your content.

To solve this problem, you have to indicate that your site supports the AJAX crawling scheme by adding a hashbang to your URLs, e.g.: 
	
	mygreatsite.com/#!/coolpage

The hashbang indicates the the web bots of the world that your site is AJAX crawlable, and the bots will respond by making a request to your application using the `_escaped_fragment_` syntax. 

	mygreatsite.com/?_escaped_fragment_=coolpage
	
It's important to note that you'll need to URL unescape any pages you send to the bots, so the URL you send ends up looking like: 

	mygreatsite.com%2F%23%21%2Fcoolpage
	
When your server sees the escaped fragment syntax, its job is to respond with an HTML snapshot of the content instead of the normal page. 