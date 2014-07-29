# Common Enumerable Techniques

There's a good reason you're writing Ruby Wrong(tm). One aspect of good code is that it is easy to understand the intent of at a glance, and easy to change. This is one of the most difficult problems to solve, particularly because it's easy to write code, but much harder to read and maintain it. The techniques I want to talk about today fall under a common header: Expanding your programming vocabulary to include shorthands for common techniques that programmers apply over and over again, so that it's easier for other programmers to understand your intent quickly. 

How many times have you seen or written code like this?

	def capitalize(array_of_strings)
		new_array = []
		array_of_strings.each do |string|
			new_array.push(string.capitalize)
		end
		return new_array
	end
	
Even though this code was stupid simple to write, it takes quite a bit of cognitive overhead to read. Okay, we're initializing a new collection, then we're looping through each provided string, capitalizing it, pushing it into the new array, and returning the new array. Even if another programmer _thinks_ they know what you're code is up to, they have to check it just to be certain.

The problem with the code above is that loops are too generic. There are many kinds of loops, and if you don't tell your readers what kind yours is (yes, you will have readers of your code, including possibly the most important one--Future You), then they will have to go through the entire process of your loop in order to understand it. 

You may be thinking "But my code is a special snowflake. My logic is complex, and cannot be reduced to common operations." I will show you that it can--most of the time--and when it can't, then it will be even easier for other programmers to spot what's particularly challenging about your code. 

What you may not know is that operation we performed above--taking a collection of things and transforming it into a collection of related things--has a name: Map.

	def capitalize(array_of_strings)
		array_of_strings.map { |string| string.capitalize }
	end
	
You might argue that it's not easier to read for a programmer that doesn't know what a `map` is. But I'd argue right back that not learning `map` is like stopping learning new words when you're in kindergarten. If your teachers let you skate by all the time saying that you "read a good book" instead of helping you to be specific about why the book was good, then you probably wouldn't have very much complexity to your thoughts. Yes, you can accomplish most every collection problem with generic loops, but as soon as the complexity increases, you'll be spending a lot more of your time reading old code than writing new code. Just take another example:

	def parents_of_students_with_a_averages(students)
		selected_students = []
		
		students.each do |student|
			selected_students.push(student) if student.has_a_average?
		end
		
		parents_of_selected_students = []
		
		selected_students.each do |selected_student|
			parents_of_selected_students.push(selected_student.parent.full_name)
		end
		
		return parents_of_selected_students
	end
	
The code above doesn't even do that much. In less general terms:

	def parents_of_students_with_a_averages(students)
		students.select(&:has_a_average?).map(&:parent).map(&:full_name)
	end
	
The second version is much easier to read and reason about at a glance. In fact, it sounds like natural English. Because it's so easy to read, it makes it much harder for bugs to hide. Let's say you made a mistake in your code:

	def parents_of_students_with_a_averages(students)
		students.select(&:has_f_average?).map(&:parent).map(&:full_name)
	end
	
Now it should be pretty apparent that there's a difference between the name of the method (`with_a_average`) and the operation we performed (`has_f_average`). In the first version this flaw would be lurking deep within the code. It might even have been difficult to spot in the second version. That one letter difference is pretty subtle. But because the subtleties of real programming challenges are often even more nefarious than in the example, the benefits of improving readability are dramatic. I hope you'll agree. If you now see the logic behind this article, let's look at the common techniques for operating over collections. 

## Enumerable Boolean Queries

#### The Problem:

You want to know whether `some`, `any`, `all`, `none`, or `one` of the things in your collection meet some given criteria. They're called "Boolean Queries" because they always return true or false.

#### The Loop Version:

So hard to read. Hope it's sinking in why you want to use these techniques:

	class Battleship
		def game_over?
			game_over = false
			
			players.each do |player|
				player_lost = true
				
				player.ships.each do |ship|
					player_lost = false unless ship.sunk?
				end
				
				game_over = true if player_lost
			end
			
			return game_over
		end
	end

#### Using Your New Vocabulary: 

In Ruby, use the methods named in the Problem section. 

	class Battleship
		def game_over?
			players.any? { |player| player.ships.all?(&:sunk?) }
		end
	end
	
This code reads like the standard English version of the problem domain: A game of Battleship is over if all the ships of any player are sunk.

I should also mention `include?`, which does what it sounds like:

	class User
		def fan_of_musician?(musician_name)
			favorite_musicians.include? musician_name
		end
	end

## Filtering & Selecting

#### The Problem: 

You want to reduce a list to only those items that meet given criteria, or the first item that meets given criteria.

#### Using Your New Vocabulary:

Since you've already seen the loop version, let's look at `find`, `detect`, `find_all`, `select`,`filter`, `reject`, and `grep`.

`find` and `detect` (synonyms) find the first match:

	nums = [1, 2, 3]
	nums.detect { |n| n > 1 }
	>> 2
	
`find_all`, `select` and `filter` (also synonyms) select all:

	nums = [1, 2, 3]
	nums.select { |n| n > 1 }
	>> [2, 3]
	
`reject` is the inverse of a select:

	nums = [1, 2, 3]
	nums.reject { |n| n > 1 }
	>> [1]
	
`grep` is usually used for what is essentially its Unix synonym, pattern matching:

	fun = %w(waterslides sandwiches parks dogs doges friends)
	fun.grep /dog/
	>> ["dogs", "doges"]
	
	fun.grep /w/
	>> ["waterslides", "sandwiches"]
	
	fun.grep /^w/
	>> ["waterslides"]
	
It can also be used for more powerful things:

	misc = [1, 2, "fun", 3, "doges", 4]
	misc.grep String
	>> ["fun", "doges"]
	
	(1..75).grep(1..5)
	>> [1, 2, 3, 4, 5]
	
With this vocabulary you have immense flexibility and expressiveness. You can even find some pretty crazy answers without blowing up your computer:

	nums = (1..Float::Infinity)
	nums.lazy.map { |n| n*n }.first(5)
	>> [1, 4, 9, 16, 25]
	
Good luck iterating from 1 to Infinity without this technique :)

### Organizing Results

You have a set of data that you want to arrange into subsets. Sometimes you want to group on a given attribute, which becomes a hash key. Sometimes you want an array of items that contains a sub-array of items for which a given query is true, and another for which it is false.

### Using Your New Vocabulary

`group_by` does what it sounds like:

	students = [corey, topanga, harry, hermione]
	students.group_by { |student| student.teacher.name }
	
	{
		"Mr. Feeny" => [corey, topanga],
		"Professor Snape" => [harry, hermione]
	}
	
`partition` splits into sets of true/false for a given test:

	nums = [1, 2, 3, 4]
	nums.partition { |n| n > 2 }
	>> [[3, 4], [1, 2]]

