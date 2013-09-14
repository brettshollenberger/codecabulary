# Angular MVC

AngularJS is a hard language to learn, particularly because its authors use the language's jargon without defining them; it's the aim of this article to clear up how Angular works.

Angular is intended to ease the pain of creating single page applications, wherein different controller actions need to manage different segments of the page at the same time. 

Let's say we have a RESTful application that's managing Project creation on a single page, and we want to list all projects, make them available for edit, and allow our users to create new projects and add them to the list. To deal with these competing concerns on the same page, Angular uses the concept of `scope`--the context in which Javascript variables exist and operate. If one `div` is managed by one controller, and a sibling `div` is managed by another, they can both create a `projects` variable referring to different sets of projects without fear of collision. Both sets of `projects `operate within separate scopes, and are independent of one another. 

Scopes can also utilize Javascript's prototypal inheritance (and usually do, but don't always as we'll see in a moment). When we declare a new Angular application, we wrap the part of the page we want Angular to manage within the `ng-app` directive, by putting it on the `<html>` element or a `<div>` element, for example, and this creates the `rootScope`. In HTML, we already have the concept of "inheritance" (however loosely defined), wherein we can understand nodes in the DOM as parents, children, and siblings. The `rootScope` is the parent of everything in a given Angular application, and all new scope objects created within the application will be descendants of `rootScope`, which means that any properties defined on `rootScope` will be available in child scopes.

#### Creating Scopes in Angular

Scopes are used to manage the Javascript context--which objects and methods are currently available for our template to work with, and which do we need to do the job of a certain section? As in other frameworks, like Rails, we can explicitly declare a new context by declaring a new controller or controller action to manage a particular resource.

		<div ng-controller="ProjectsController">
			{{projects}}
			<button ng-click="newProject()">Create a New Project</button>
		</div>
		
		<div ng-controller="ActivitiesController">
			{{activities}}
			<button ng-click="newActivity()">Create a New Activity</button>
		</div>
	
Even if these two `divs` were siblings on the page, occurring right next to one another as we see above, they wouldn't have access to one another's objects and methods. The ProjectsController could display all projects and assign a button to a function that creates a new project, but it couldn't display activities or assign a function to create a new activity. The `activities` object doesn't exist in its scope, and neither does the `newActivity()` method. The two are totally isolated. But, if we create a new child scope under the ProjectsController, it will have access to all methods and objects defined on the ProjectsController:

	<div ng-controller="ProjectsController">
		<div ng-repeat="project in projects">
			<div ng-click="showProject(project)">{{project.title}}</div>
		</div>
	</div>
	
The `ng-repeat` directive creates a new child scope for each project in the `projects` array, where the new `project` variable refers to the current project in the iteration. `project.title` refers to the title property on that given project. `ng-repeat` creates new scopes for each child implicitly--we can't tap into this particular scope to define _new_ methods or properties on each project as we can when we define `ng-controller`, but 