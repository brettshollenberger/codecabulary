# Procs

#### You can't pass methods into other methods.

#### You can pass prods to methods, and methods can return procs. Procs are objects, methods aren't.

		say_cool = Proc.new do
			puts "Cool."
		end
		
		say_lame = Proc.new do
			puts "Lame."
		end
		
		def conditional(a, b, c)
			if a > 20
				b.call
			else
				c.call
			end
		end
		
		conditional(100, say_cool, say_lame)
		> Cool.