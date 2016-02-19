# Angular Testing: Mocking $httpBackend

In Angular unit tests, we want to test individual units of code, not their interaction with the server. To do so, `angular-mocks` provides us with a fake HTTP backend service `$httpBackend` that allows us to respond to the HTTP requests that our application will make in known ways.

`$httpBackend` works by hijacking the dependency injection chain to inject our fake `$httpBackend` where would normally inject the real thing. 

In production, `$httpBackend` responds to request asynchronously. We can't test asynchronously--we need our data to be available to write assertions against it, which is one of the reasons testing AJAX applications can be difficult, but one of the things that Angular makes easy. In order to handle asynchronous calls synchronously, we first set up the expectations for `$httpBackend` (we describe how `$httpBackend` should respond), and then we call the `flush()` method to ensure that `$httpBackend` responds with our data. 

For example, let's assume that we have a directive that is going to request a template. Here's a real example I've used in production:

	angular
  		.module('app')
  		.directive('lsNav', function() {
    			return {
			      restrict: 'A',
			      scope: {},
			      templateUrl: 'views/components/nav/nav.html',
			      controller: ['$scope', function($scope) {
			      	... controller logic ...
			      },
			      link: function(scope) {
			      	... link logic ...
			      }
			}
		});
		
In order to respond to this template request, we have to wire up `$httpBackend` to respond to the request with the template to avoid making requests of our server:

	var nav, scope, $compile, $httpBackend, template;

	beforeEach(inject($rootScope, _$compile_, _$httpBackend_) {
		scope   = $rootScope.$new();
		$compile = _$compile_;
		$httpBackend = _$httpBackend_;
    		nav      = '<nav ls-nav></nav>';

		  // We have to mock out $httpBackend to respond to the directive's request
		  // for a template with a simple, known template. This only sets up the response;
		  // we still need to call $httpBackend.flush() after we setup the controller to
		  // ensure the template gets filled in.
		  $httpBackend = _$httpBackend_;
		  template     =  '<div ng-repeat="link in nav.links">' +
		                      '<div ls-nav-link="link"></div>' +
		                    '</div>';
		  $httpBackend.expectGET('views/components/nav/nav.html').respond(template);
		  navLinkTemplate     = '<a href="{{href}}">{{text}}</a>';
		  $httpBackend.expectGET('views/components/nav/nav-link.html').respond(navLinkTemplate);
    
		  // Finally, we bootstrap the template by compiling the directive into HTML, which
		  // returns the NAV directive's link function. The link function receives a single
		  // argument, a scope, which we created earlier. We then flush the template from
		  // the backend on the bootstrapped template to fill it in. We call scope.$digest()
		  // to ensure all changes that have been made on the scope object are propagated
		  // throughout the application; we have no fear of an asynchronous call not being
		  // fulfilled by the time we get to our expectations.
		  $compile(nav)(scope);
		  $httpBackend.flush();
		  scope.$digest();
		    
Now `$httpBackend` will respond with our template, and we can continue by writing our assertions for the directive. Notice that the template it responds with itself calls another directive that makes another template call, and we need to stub that out as well.

#### Setting up httpBackend expectations

Above, we setup our `$httpBackend` expectations for GET requests using `$httpBackend.expectGET('urlToGet').respond(template)`. Similarly, we can expect POST, PATCH, PUT, DELETE, HEAD, and JSONP requests. Each of these methods has a different signature that allows us to respond to the request in different ways.

`expectGET` `expectHEAD`, and `expectDELETE`:

* URL - the URL we expect our directive to request
* Headers (optional) - HTTP Headers

`expectPOST`, `expectPATCH`, `expectPUT`:

* URL
* Data (optional) - The request body or function that receives the data string and returns true if the data string is as expected or a JSON object
* Headers (optional)

`expectJSONP`:

* URL

#### Responding to httpBackend expectations

The next step in setting up our `$httpBackend` expectations is setting up our predetermined responses to them. Our `expect()` method calls return a requestHandler that has a single function: `respond()`.

Our respond function can take two forms: one replies with a response code, response data, response headers, or all three; the other is a function that returns an array that contains the code, data, and/or headers. 

 	$httpBackend.expectGET("/v1/api/current_user" )
 		// Respond with a 200 status code
		// and the body "success"
		.respond(200 , 'Success' );
		// Or only return data
		.respond("Fail" );
		// Or only headers
		.respond({'X-RESPONSE': 'Failure' });
		
	 $httpBackend.expectGET("/v1/api/current_user" )
		// Respond with a 200 status code
		// and the body "success"
		.respond(function (method, url, data, headers) {
			return  [200 , "DATA" , {"header1": "Header1" }];
		});
		
#### Using `when` instead of `expect`

`$httpBackend` also has a `when` method that differs from `expect` in that:

* It does not set up an expectation
* It sets up a fake backend for the app primarily to return fake data
* A response is `required` by a `when` function, whereas it's not from `expect`
* Every single request that matches the URL definition can be handled by a single `when` definition

These properties make `when` great for establishing backend mocks common across multiple tests. 
