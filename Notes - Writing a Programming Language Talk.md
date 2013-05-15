### Notes - Writing a Programming Language Talk

* Why: If you understand how a programming language works, Ruby no longer seems mystical; you can commit to Ruby

### Four easy steps:

* Tokenize
>Break up pieces into component parts; labeling; taxonomy

Tools: Ragel

* Parsing 
>Take each token and make a tree. Give each token contextual meaning. 

E.g. BinaryExpression has a left term, a right term, and an operator

Tools: Racc - Ruby port of Yacc (Yet another compiler compiler); gem for racc

Backus-Naur Form - 

		program: statement { result = Root.new(val[0]) }
		statement: term OPERATOR term {
		result = BinaryOperator.new(
			val[1]
			val[2]
			...
		)

* Interpret
> Get to the sub-language: the simple form of what you want to express; produces byte code
* Execute
> Virtual machine: A low-level computer system to run your code