# Advanced Angular Directives

We can register directives with Angular's dependency injection system via a method on the `$compile` provider that takes a name and a factory function:

	ng.$compileProvider.directive = function registerDirective(name, directiveFactory) { ... };
	
Factory functions are functions that return new objects, much like constructor functions. The difference with the Factory Method Pattern is that it is used to decide which class to use in a given scenario--meaning that a factory may return different objects based on given parameters (parameterized factory method) or compile a different object to return composed of objects based on given inputs (abstract factory method). The truth is with Angular directives (and with the Factory Method Pattern in general) is that you often won't need to use a factory over a more generalized constructor function, but the composition of all Angular directives requires a factory function, so for most intents and purposes, we'll just need to know that our directives should return an object, but we can keep in mind that as long as a directive returns an object, it doesn't matter what intermediate work is done in the directive, and we can thus create more robust directives using the FMP. 

In Angular-specific terms, the `$compile` provider's `directive` method calls the `$provide` service's `factory` method, which registers the directive with the `$inject` service so that it can be injected as a dependency throughout the rest of the application with the suffix 'Directive', e.g.  a directive named slider gets registered as sliderDirective and a directive named navigation gets registered as navigationDirective; these could theoretically be injected into your controllers or other services if you needed access to them, although this is rarely how directives are used in the wild. 

#### Returning Objects versus Returning Functions

The directive factory function can return an object or a function. 