Ruby Command Line Switches
========================
When you write a Ruby program, you'll run it from your command line. The command line allows you to select a few special options, called command-line switches, which perform actions on your Ruby code.

The -c flag (Check Syntax)
========================
```
ruby -c example1.rb
```
The -c flag checks the syntax of the program file without executing the program. If everything's looking good, you'll see

```
Syntax OK for example1.rb
```

Remember (for all you that don't have a degree in English Literature), syntax is just the rules for constructing sentences (or lines of Ruby in this case), whereas semantics are the meaning behind the words. In English, you could say "The verb crumpled the milk--terribly." And your sentence would be syntactically correct. But it wouldn't make a whole lot of sense, would it? The -c flag is only checking for syntax, not semantics; so be sure to perform some other tests on your code like:

The -w flag (Warning Mode)
========================
```
ruby -w example1.rb
```

The -w flag runs the Ruby interpreter in warning mode, and will draw your attention to code that, while syntactically correct, is logically or stylistically suspicious. Warning mode is more verbose than running the Ruby interpreter normally; though the normal interpreter will complain about some things, the warning mode interpreter will be the "English Nazi" you always hated, but will love in this case. You love the just punishment of warning mode!

The -cw combination (Check Syntax + Warning Mode)
========================
```
ruby -cw example1.rb
```

This glorious combination of flags (both the -c and -w flags combined) will both check your syntax without running the program and also give you semantic warnings. Good stuff. Any flags can be combined in this way (-ve is another common combination we'll explore below), but these two are the most common combos. They also earn you bonus points in coding heaven, so there's that.

The -e flag (Execute Literal Ruby Script)
========================
```
ruby -e 'puts "Whatever ruby -e wants to put."'
```

The -e flag connotes that literal Ruby script follows and should be run. Most flags are executed on Ruby files (as shown above in the example1.rb files). In those examples, example1.rb contained Ruby code that was passed to the Ruby interpreter to run. With the -e flag, you're passing in the code directly to the interpreter for a result.

The -v flag & -e flag combination (Verbose + Literal)
========================
-e is most often used with the -v flag (which displays the version of Ruby a code will run through, and also runs in warning mode without the need to declare the -w flag), for educational purposes. In a forum, someone might illustrate that a feature changed between Ruby 1.8.6 and 1.9.1 by running both versions and sharing the output. For example:

```
ruby186 -ve "puts 'The end of the world'.start_with?('T')"
ruby 1.8.6(2007-03-13 patchlevel 0) [i686-darwin8.10.1]
-e:1: undefined method 'start_with' for "abc":String (NoMethodError)

ruby -ve "puts 'The end of the world'.start_with?('T')"
ruby 1.9.1 (2008-12-30 patchlevel-0 revision 21203) [i386-darwin9.5.0]
true
```

As we see in the latter case, the Ruby method 'start_with' evaluates to true; in the former case, Ruby complained that start_with didn't exist, which it didn't in 1.8.6. Using the -ve combined flag allows us to share the output of different Ruby versions with others in productive ways.

The -l flag (Add a newline after string outputs)
========================
```
ruby -l example1.rb
```

The -l flag forces Ruby to add a newline (\n) after every string literal output. The effect of this change is to transform all print statements into puts statements (puts aka put string automatically adds a newline to its output; print does not).

The --version flag (Print Ruby version)
========================
```
ruby --version
```

This flag prints the current version of Ruby and then exits. Like the -v flag, but without any code to execute on, and (inherently) it won't run in warning mode (since it's not running on anything).

The -h or --help flag (Chduh, man)
========================
```
ruby --help
```

These switches give you a table listing all the command-line switches available, and summarizing what they do, much like this document, but a little less verbose. This doc hopes to add a little clarity to the terse, mechanical goodness of ruby --help.
