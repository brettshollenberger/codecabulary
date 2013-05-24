# String Literals

> This was literally the most beautiful and moving thing I’ve ever heard. <br/>
> -- Chris Traeger, Parks and Recreation

String literals are precisely that: the exact sentences, words, or characters you type in between two quotes. Literally. A string literal in Ruby could be:

```
  'Single quoted'
```

Or

```
  "Double quoted"
```

Quite simple, really. The difference between single quotes and double quotes is just how literal your string takes you for. Single quotes take you very, very literally.

```
  puts 'Test\ntest'
  #> Test\ntest
```

Quite literal, no? Double quotes will handle both ASCII escaped characters (like the newline character we tried to add in the first example), and interpolation (e.g. we can use variables as stand-ins for the strings they represent).

```
  puts "Test\ntest"
  #> Test
  #> test
```

Notice how the newline character is escaped in the double-quoted version. A little less literal, but quite a bit more pragmatic. Let's look at uses for interpolation. Suppose you have an array of shopping items:

```
  groceries = ['ice cream', 'chicken tenders', 'instant soup']
```

A true bachelor's delight. Using interpolation, we can loop over these items to produce some snazzy new strings.

```
  groceries.each do | item |
    puts "I sure do need #{ item }"
  end

  #> I sure do need ice cream
  #> I sure do need chicken tenders
  #> I sure do need instant soup
```

That was easy. The code examples here have followed the recommendations of the [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide#strings), which we highly recommend you get into so you can follow the best practices of the Ruby world, and more easily read others' code.




