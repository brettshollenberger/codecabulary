ri and RDoc
========================

ri (Ruby Index) and RDoc (Ruby Documentation) provide documentation about Ruby programs. Both include command-line tools that can be installed using [Ruby Version Manager](https://github.com/brettshollenberger/codecabulary/blob/master/ruby/rvm.md). You can check out our RVM page for info on installing RVM.

```
  #> rvm docs generate
```

Once ri is installed, you can use it to request information about Ruby. Let's say you just saw string.upcase used in a StackExchange article. You can quickly look up info on the .upcase method by typing:

```
ri String#upcase
```

The call to ri will return documentation, like this:

The basic format for looking up documentation through ri is:

For [instance methods](https://github.com/brettshollenberger/codecabulary/blob/master/ruby/instancemethods.md):
```
  #> instance#methodname
```

For [class methods](https://github.com/brettshollenberger/codecabulary/blob/master/ruby/classmethods):
```
  #> class::methodname
```
˜˙
