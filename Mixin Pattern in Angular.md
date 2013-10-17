# Mixin Pattern in Angular

Mixins are a set of reusable functionality that can be included in classes to provide multiple inheritance. For instance, Ruby core defines both hash tables and arrays using a set of common functionality for enumerable objects via the Enumerable module mixed into both classes. The methods for creating mixins vary depending on language, and today we'll look at this pattern in Javascript's Angular framework.

#### angular.extend

Angular provides a function called `extend`, which copies the properties and methods of a source object into a destination object if properties of the same name do not already exist in the destination. Let's take a look at how we can create some view helpers in Angular to provide common functionality needed across all views in a sample application. These methods will help us set page meta information from our controllers, including titles, descriptions, and og: properties for web crawlers like Facebook. Ruby on Rails uses this mixin view helper pattern by default, and we can duplicate it in Angular with minimal effort. 

1) Register a service with Angular's dependency injection framework:

The methods we want to mixin can be defined in objects that can be injected into any other service, controller, directive, etc using services. Services are factory functions, meaning they return objects, and we'll mixin the properties of this object into our root-level controller so the properties will be available across our application:

	var viewHelpers = angular.module('viewHelpers', []);
	
	viewHelpers.factory('metaHelper', function() {
	
		var base = $location.absUrl();
	
		var metaDefaults = {
		      metaType: "website",
		      metaName: "My Great Page",
		      metaTitle: "Home | My Great Page",
		      metaDescription: "Beautiful websites",
		      metaImage: base + "app/images/favicon/my-favicon.jpg",
		      metaUrl: base
		 };
		 
		return {
			title: function(pageTitle, baseTitle) {
			        baseTitle = typeof baseTitle != 'undefined' ? baseTitle : "Rootstrikers";
			        fullTitle = typeof pageTitle != 'undefined' ? pageTitle + " | " + baseTitle : baseTitle;
			        $('title').text(fullTitle);
			        $('meta[property="og:title"]').attr('content', fullTitle);
			      },
			description: function(description) {
			        description = stripHTML(description) || metaDefaults.metaDescription;
			        $('meta[property="og:description"]').attr('content', description);
			        $('meta[name="description"]').attr('content', description);
			 },
			 image: function(url) {
			        metaImage = url || metaDefaults.metaImage;
			        $('link[rel="image_src"]').attr('href', metaImage);
			        $('meta[property="og:image"]').attr('content', metaImage);
			 },
			 url: function(url) {
			        metaUrl = url || metaDefaults.metaUrl;
			        $('meta[property="og:url"]').attr('content', metaUrl);
			 }
		}
	});
	
2) Mixin our metaHelper:

To mixin our helper, we'll inject it into a controller at the highest-level in the DOM, which our other controllers will inherit from prototypically. We'll mix these methods into the `$scope` object at this level, and each new scope will be able to access these helpers via the lookup chain, unless we overwrite one of the methods exposed on scope in a child scope.

	var app = angular.module('app', ['mixins']);

	app.controller('MainCtrl', [
		  '$scope',
		  'metaHelper',
		  function($scope, metaHelper) {
		  	angular.extend($scope, metaHelper);
	}]);
	
Voila! Now the properties defined in metaHelper have been copied to the `$scope` object, and we can access them there.

3) Proof is in the tests:

Using Angular.mocks and Jasmine, we can test that our functions have been mixed in: 

	describe('Angular mixins', function() {

	  	var $controller, $scope, metaHelper;
	
	  	beforeEach(module('app'));
	
		beforeEach(inject(function(_$controller_, _$rootScope_, _metaHelper_) {
		    $controller   = _$controller_;
		    $scope        = _$rootScope_.$new();
		    metaHelper = _metaHelper_;
		    $controller('MainCtrl', {
		      metaHelper: metaHelper,
		      $scope: $scope
		    });
		}));
	
		  it('mixes in those attributes', function() {
		    expect(typeof $scope.title).toEqual('function');
		  });

	});
