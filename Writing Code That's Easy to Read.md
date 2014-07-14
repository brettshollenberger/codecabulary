# Writing Code That's Easy to Read

Imagine you are the owner of a Family Fun Center, complete with miniature golf course, go karts, arcade, and snack bar. You have a teenage staff who are quite good at following directions, but let's not kid ourselves--is not very good at making decisions on their own. For many, this is their first job. They are the novices and advanced beginners on the Dreyfus scale--with some of the advanced kids, the 17-year-old supervisors, as the competents and proficients. Believe it or not, your job is a lot like working with a team of programmers. 

When you design a new piece of code that describes how popcorn will be popped, you are designing the protocol that the fourteen-year-old snack bar guy is going to use--word for word. The more conditional logic you  use, the more difficult it's going to be for him to move up the ranks of the Dreyfus scale, because it's harder to understand the _why_ of the situation when you're only presented with the _how_:

	class Popcorn
		def pop
			until @kernels.brownness_percent > 35 || @kernels.internal_temp > max_heat
				... imagine equally complex code ...
			end
		end
	end
	
Just what the heck does `brownness_percent` or `internal_temp` have to do with anything wonders the new reader of the popcorn program? It's a good question, and one that you shouldn't hide the answer to:

	class Popcorn
		def done?
			@kernels.brownness_percent > 35 || @kernels.internal_temp > max_heat
		end
		
		def pop
			until done?
				...
			end
		end
	end
	
Aha! Done is a much more understandable heuristic to we humans, even if the computer doesn't give a crap what `done` means. Computers are quite good at making sense of spaghetti code, but that 14-year-old kid isn't going to experiment with the popcorn-popping process with such scary-sounding words like `browness_percent` standing in his way. The process works; why screw with it?

The important point is this: I'm not even sure that's what constitutes popcorn `done`ness. Today, the algorithm is acceptable, but Future Me is going to know a lot more about running a snack bar, and the 14-year-old that runs it day-to-day is going to know even more than me, even though I'm the one who originally wrote the protocol. The code will be better if it is easier to understand my original intent. If popcorn kid wants to experiment with optimal doneness, it will be much easier for him to do if he knows _why_ that initial condition is in the code. I've given the rest of the team insight into my mind: it should be very easy to see when I've made a silly mistake, or a mistake that doesn't meet the business concerns of my application.

Ideally, even my project manager could read my code and determine whether or not it would function the way the business needed. This can be made possible if we really nail down domain-specific concepts into little methods. Decomposing conditionals is one way to help your team stay assertive about one another's code. 
			
