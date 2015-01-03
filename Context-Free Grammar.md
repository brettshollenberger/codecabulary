# Context-Free Grammar

In natural language and programming languages alike, we have a common problem: How do we get a computer to understand a set of instructions in a given language? Developers may sometimes take it for granted that a programming language is capable of creating what the abstractions we dream up--after all, at the end of the day, a text in some programming language is still just a string of text. 

The problem of parsing strings of text in a given language into the semantic meaning of the statements has been solved with _context free grammars_, which, informally, specify a set of rules to reduce the text into its constituent parts in the programming language. 

Parsers typically operate on streams of _tokens_ as opposed to streams of _characters_--that is, the streams they're given have already been parsed into keywords, constants, identifiers, and the myriad other pieces in a programming language. It's up to the parser to deduce _how_ these pieces are being used--are they method calls? Variables? Blocks that ought to create closures?

To understand how production rules work, let's take a look at a very simple context-free grammar for an English sentence. The `->` operator in the following examples is taken to mean "consists of", as in "sentence consists of a noun_phrase followed by a verb_phrase, in that order." The pipe character (`|`) is used to indicate options, as in "a noun phrase consists of a noun, or an adjective and another noun phrase," where we can see the recursive definition would allow for as many adjectives as we want to precede the noun.

	sentence -> noun_phrase verb_phrase
	
	noun_phrase -> noun | opt_adjective noun_phrase
	
	verb_phrase -> verb opt_direct_object
	
	verb -> ... | some_complex_definition | ...
	
	opt_direct_object -> noun
	
	noun -> I | You | He | She | It | We | They | ...
	
This set of production rules constitutes the entirety of our simple English language. We can use it to parse streams of tokens like this:

	| Step            | Parsed Tokens                          | Remaining Token Stack | 
	| --------------  |:-----------------------------------------:|:--------------------------------:|
	| 0                 |                                                   | We love friends               |
	| 1                 | We                                             | love friends                    |
	| 2                 | Noun                                          | love friends                    |
	| 3                 | Noun love                                   | friends                            |
	| 4                 | Noun Verb                                  | friends                            |
	| 5                 | Noun Verb friends                      |                                        |
	| 6                 | Noun Verb Noun                        |                                        |
	| 7                 | Noun Verb Opt_Direct_Object   |                                        |
	| 8                 | Noun Verb_Phrase                    |                                        |
	| 9                 | Noun_Phrase Verb_Phrase      |                                         |
	| 10               | Sentence                                   |                                         |
	
We see in the example above that we continue making replacements after we've popped off a few more tokens from the stack, and have deduced the complete meaning of the tokens in tandem. Certain pieces like the initial `Noun` later realize that they could be reduced to `Sentence` if changed to `Noun_Phrase` after we see `Verb_Phrase` appear. In this way we use our production rules to look ahead, and see the best possible reduction scenario.

The "context-free" part of "context-free grammar" is also a simple concept--it means that we can reduce our tokens using the production rules regardless of what other tokens surround them (though of course often the sequences are reduced to another simpler step together). 