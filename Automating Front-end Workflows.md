# Automating Frontend Workflows

#### Developing Your Workflow with Grunt

Grunt's website features tons of pre-packaged npm modules that can be run from the command line, and built into a workflow to remove the tedium from repetitive tasks like minification, concatenation, running unit tests, etc. To write a Gruntfile, we follow this basic workflow:

1) Load the task:

	$ npm install task-name --save
	
This will not only install the task, but it will update it in your `package.json`. 

In our Gruntfile, we load the task with grunt's `loadNpmTask` method:

	module.exports = function(grunt) {
		grunt.loadNpmTasks('grunt-contrib-concat');
	};
	
2) Configure the task:

Each task will have a set of configuration options that you're able to set. For `grunt-contrib-concat`, we could ask that any any Javascript files in `src/js` and its subfolders get concatenated together and output as `dev/app.js` using the `globstar`, which will recurse through subdirectories, and the globbing wildcard matcher `*`, which will match any filename:

	grunt.initConfig({
		concat: {
			js: {
				src: ['src/js/**/*.js'],
				dest: 'dev/app.js'
			}
		}
	});
	
3) Add the task to the workflow:

	grunt.registerTask('default', 'concat');
	
This adds concat to the default task (what happens when you just run Grunt). With any name other than default, we run the grunt task as:

	$ grunt task-name
	
