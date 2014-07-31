# Ruby Hook Methods

When we `include` or `extend` modules, we gain access to hook methods that let us tap into the including or extending class:

	module Validatable
		def self.extended(klass)
			klass.class_eval do
				def self.validates(name, &validation)
					define_method "#{name}=" do |value|
						raise "Invalid attribute" unless validation.call(value)
						instance_variable_set("@#{name}", value)
					end
					
					define_method name do
						instance_variable_get("@#{name}")
					end
				end
			end
		end
	end
	
	class Person
		extend Validatable
		
		validates :age do |age|
			age >= 21
		end
	end
	
	person = Person.new
	
	person.age = 18
	>> RuntimeError: Invalid attribute
	
	person.age = 21
	21
	