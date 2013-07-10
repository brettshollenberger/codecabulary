# Impersonation Attacks

	POST to /users/8/comments  -- susceptible to impersonation
	
	Use current_user.comments in that POST
	
It's okay to GET comments without moving through the current_user object