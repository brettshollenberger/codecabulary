# Angular Isolate Scopes

In many languages, we use modules to break up code into reusable chunks. Modules act as namespaces--containers for a set of identifiers; in Javascript we often call this the execution context. Encapsulating a component's pieces generally ensures that there are standard, known ways into the object (its API, or perhaps a constructor function), and thus known entities that it will be interacting with (provided its concerns are well-defined by the Law of Demeter).

For instance, a class is another type of namespace that houses class and instance methods--in this example, let's say we have an HTML document formatter that requires knowledge about a document to format:

	class HTMLFormatter < TextFormatter
		attr_accessor :document
		
		def initialize(document)
			@document = document
		end
		
		def format
			@document.each { |line| markup(line) } 
			@document
		end
		
		def markup
			...
		end
	end
	
In the case of the HTMLFormatter, we expect it to require a document, and for it to output a document when it's done formatting. It has a well-defined and predictable interface. 

With Angular, we can apply the same encapsulation and public interface design to reusable page components called directives. 