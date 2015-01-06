# Ruby Tokenization and Parsing

Ruby code is just a string. No matter how magical it looks, and no matter how well your text editor may format it and recognize its different components, at the end of the day, it's just text in a file. So how on earth does a computer know what to do with it?

Ruby has a three step process for turning your code into something its interpreter (YARV) understands:

## 1) Tokenize - Turn the string into recognizable components of the Ruby language. For instance:

	def fun?
		true
	end
	
Produces the following output when run through Ruby's `Ripper`:

	[[[1, 0], :on_kw, "def"],
	 [[1, 3], :on_sp, " "],
	 [[1, 4], :on_ident, "fun?"],
	 [[1, 8], :on_nl, "\n"],
	 [[2, 0], :on_kw, "true"],
	 [[2, 4], :on_nl, "\n"],
	 [[3, 0], :on_kw, "end"],
	 [[3, 3], :on_nl, "\n"]]

We see from this example that it splits up keywords (like `def`, `true`, and `end`), spaces, new lines, and identifiers (like `fun?`). It identifies their line and column numbers, their type, and the token itself. This is how Ruby understands which components it's working with in a given file.

`Ripper`'s source code is a loop that reads in a single character at a time and processes it based on what character it is. For instance:

	10.times do |n|
		puts n
	end
	
When Ruby begins evaluating the code above, it starts at position zero, the number `1`. It recognizes `1` as the start of a number, and proceeds to the next character, a `0`. Ruby will continue to proceed until it reaches a non-numeric character, so when it reaches `.`, it continues, tentatively, because it recognizes that `.` could mean that it is actually parsing a `Float`. As soon as it reaches `t`, it stops iterating, because it recognizes a non-numeric character. Now, it steps back to the `.`, and parses the numeric characters that it found (`1` and `0`) into the number 10 (represented by `tINTEGER` internally). 

It continues parsing, recognizing the `.` as a method call on the number 10, `times` as an identifier, `do` as a keyword, and so on. 

Once it has tokenized our code, how does it know what to _do?_ The arrangement of tokens defines the semantics of our program, and since tokens can be arranged quite flexibly in Ruby, there must be some step to figure out the meaning of the arrangement of the tokens we have right now, right?

## 2) Parsing

Parsing is the step that takes our Ruby tokens and decides on their meaning in our program. It groups the words into the "sentences" and "paragraphs" that make sense to Ruby. The `Ripper` will not do any analysis--it simply wants to know what tokens exist and hands those tokens off to the parser. 

Like many programming languages, Ruby's parser is defined using a `parser generator`. The code read by the parser generator defines a set of grammar rules and patterns that are compiled into the actual parser. The most popular parser generator is `YACC`, aka `Yet Another Compiler Compiler`, and Ruby uses a more modern version of `YACC` called `Bison`. The Ruby source code defines all of its syntax rules in `parser.y`, which `Bison` uses to generate `parser.c`, the actual parser for the Ruby language. 

So after our tokenization process, we have a stream of tokens. How does the parser process the incoming tokens? The answer is an algorithm known as `LALR`, or `Look Ahead Left Reversed Rightmost Derivation`, which sounds scary as all hell, but we'll break down here. imju                                                                   

* What ideas does Ruby borrow from Smalltalk and Lisp?
* What important computer science concepts underpin Ruby's core features?
* How does the team that built Ruby intend for you to use the language?
* What is a parser generator?
* What is YACC?
* What is Bison?
* How does the parser parse tokens?
* What is the LALR algorithm? How does it work?