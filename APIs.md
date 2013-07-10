# APIs

Two formats - 

	1) JSON
	
	2) XML
	
With XML  - There's much more ability to do meta work; it can get very noisy that way

More people turning to JSON - only data; key-value store

	> Programmableweb.com/apis
	
	> Like Ruby Toolbox for APIs
	
* Base choices on reputation

* Look for RESTful APIs

* Nokogiri for web scraping
	* Web scraping is frowned upon in the industry;
	* Programmatically nailing their site to acquire data; could cause them issues
* `bundle gem` like `rails new` for gems
* We define gem dependencies in the gem_name.gemspec
		
		gem.add_dependency 'nokogiri'
		
		gem.add_development_dependency 'rspec'
		gem.add_development_dependency 'vcr'
		
	> Only Nokogiri will be installed if someone downloads this gem; development dependencies are only for writing them gem.
	
* Always prefer structured data (APIs) to web crawler - you're beholden to the design changes of the team that manages that page
 	 
* Put hard dependencies in .gemspec

Nokogiri returns doc variable

doc.css('#hour00')
doc.css('#nowCond')