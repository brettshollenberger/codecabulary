# Ruby Scripting

Shell scripting can help you automate many of your tasks--automating deployment, setting up testing environments and running tests, backing up your data, performing file conversions--if your computer can do it, you can script it (seriously, a friend and I just stumbled upon FXScript, a scripting language to help Final Cut Pro users automate common video editing and conversion tasks). Today we'll look at your shell and writing shell scripts in Ruby.

## Processes

When you're running a script (some executable like `ls`, `cd`, or a Ruby script like `rake`), you've got yourself a process--a process is a program that's running in memory. Your shell itself (like `bash` or `zsh`) is a process, and processes can start child processes--this is what happens when your shell executes any of the aforementioned programs. The shell process will automatically load its startup files (like `.profile`, `.bashrc`, `.zshrc`, etc, depending on your shell), and those startup files will declare certain variables in the context of the process (like your `PATH`, the list of directories your shell checks for executable files whenever you type commands).

Each child process receives its own environment--a replica of its parent environment (in many cases, the shell). But each child process is free to live its own life--to alter variables and declare new ones--without altering the parent process. 

Let's look at a `rake` task for running tests in a `test` environment. Our script is taken from a real project that has a few requirements:

1) Run some Capybara tests. 
2) Capybara tests need a webpage to interact with in order to perform their job, so we need to run a server. In this case, we're running a Javascript server with `grunt server`.
3) Our developers may well be running a development server simultaneously while they're running their tests. Our `test` server should not interfere with their development server, or change the environment in which the development server is running. 
4) Our `run_server` task and `spec` task will inquire into their environment (using the `ENV` variable), so we should set `ENV=test` for this process. Since we're in a child process, this will not alter the parent shell process.

	namespace :spec do
	  desc "Run tests against localhost. Spawns a test server on localhost:9000 unless one is already running."
	  task :test do
	    Rake::Task["setenv"].invoke("test")
	    unless server_ready? test_server
	      Thread.new { Rake::Task["run_server"].invoke }
	      wait_for_test_server
	    end
	    Rake::Task["spec"].invoke
	  end
	end
	
	# Since our task starts a sub-process, we will not
	# modify the ENV variables of the parent shell.
	task :setenv, [:env] do |name, options|
	  ENV["ENV"] = options[:env] || "development"
	end
	
	task :run_server do |name, options|
	  sh `grunt server`
	end
	
	def test_server
	  "http://localhost:9000/login" 
	end
	
	def wait_for_test_server
	  print "Preparing your test server. This will take a minute."
	  until server_ready? test_server do
	    sleep 1
	    print "..."
	  end
	end
	
	def server_ready?(_url)
	  begin
	    url = URI.parse(_url)
	    req = Net::HTTP.new(url.host, url.port)
	    res = req.request_head(url.path)
	    res.code == "200"
	  rescue Errno::ECONNREFUSED
	    false
	  end
	end

