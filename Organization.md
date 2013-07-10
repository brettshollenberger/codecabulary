Modules are always read as folders in Rails -- as long as they're in the proper directory, you're cool

ActivityTracker
	UserStoryActivityTracker
	ProjectActivityTracker
	
	ActivityTracker::UserStoryActivityTracker.new
	
Include just mixes in methods; it merges them as if they were one class.