# Gem Locking

Bundler is a Ruby tool that resolves dependencies between your dependencies--between the external libraries ("gems") that you rely upon in your application. Where this challenge used to be solved by hand, Bundler now solves it for you, and then "locks" the results into a file called the `Gemfile.lock`. The `Gemfile.lock` is the result of the resolution of the gem manifest (the `Gemfile`). 

The `Gemfile.lock` should always be checked into version control, as it ensures the same versions of gems will be used by each developer. Additionally, it ensures that the same gem versions are used in production as the gems that were tested against in development, which helps developers avoid introducing unforeseen bugs.