# Manipulating Ruby Blocks for Fun & Profit

Blocks are ubiquitous in Ruby--and one reason why is because they can be used to control scope. Local variables can exist in the scope of a block, and that scope can be shared between methods and classes, but remain encapsulated within the block to avoid polluting the global namespace. Let's see how:

	# procs are blocks turned into objects
	# we can use them to create anonymous scopes that are called immediately
	# sharing local variables between them, but keeping them from littering the global namespace
	
	proc {
		events = {}
		setups = []
		
		Kernel.send :define_method, "event" do |name, &block|
			events[name] = block
		end
		
		Kernel.send :define_method, "setup" do |&block|
			setups << block
		end
		
		Kernel.send :define_method, "each_setup" do |&block|
			setups.each do |setup|
				block.call setup
			end
		end
		
		Kernel.send :define_method, "each_event" do |&block|
			events.each do |name, event|
				block.call name, event
			end
		end
	}.call
	
	# Now we have several methods exposed on the Kernel object, which is mixed into every object,
	# including main. However, these methods private manage state with the events and setups 
	# variables that remain local to the proc
	
	event "sky_is_falling" do
		@sky_height < 100
	end
	
	setup do
		@sky_height = 50
	end
	
	# We can also avoid polluting the global namespace by creating "clean rooms,"
	# objects whose sole purpose is to evaluate our blocks
	
	env = Object.new
	
	each_setup do |setup|
		env.instance_eval &setup
	end
	
	each_event do |name, event|
		puts "ALERT: #{name}" if env.instance_eval &event
	end