# Yeoman

Yeoman offers workflow tools:

* Yo for webapp scaffolding
* Bower for dependency management
* Grunt for building, testing, and previewing

#### Yo

Yo scaffolds your webapps with generators, much like Rails. 

1) First, install the generators you want to use via npm:

	npm install -g generator-webapp
	
The Yo generators follow this generator-<name> convention, so it should also be easy to find generators like generator-angular

2) Any of these packages prefixed with generator- can be used with the `yo` command, dropping the `generator-` portion:

	yo webapp

#### Bower

* Manages and installs dependencies
* Hosts remote packages like RubyGems to make them easier to install like `bower install query`
* Supports search of its repositories like `bower search angular` or `bower search`, which returns all repos
* Does not handle script concatenation, minification, or module loading like RequireJS or Sprockets

#### Grunt

Command line task runner for Javascript. Handles repetitive tasks like minification, compilation, unit testing, linting, etc. 

