# How To Create a Rails App
## A step by step guide to prepare your app base at Launch Academy.

By Conner Smith

First there was “rails new Foo” -- the next thing you know you’re adding “group :test, :development do gem ‘rspec-rails’ end” to your Gemfile and editing your database.yml file to reflect the correct localhost IP. While learning Rails for the first time, it’s difficult to remember these various minutiae of setting up your app; moreover, it’s even more difficult to remember which order they should be performed in. This can often mean running into some time consuming errors along the way. Here is a step by step guide I created, and very often use, to make sure I’m not missing any steps in setting up a working Rails app.

===

### The 17.5 Steps:

#### 1) rails new <name> --database=postgresql

At Launch Academy we tend to use postgresql. So to save yourself a little extra work, make sure you include the --database flag when executing rails new.

#### 2)  remove turbolinks in Rails 4.x -- for Rails 3.x, make sure you remove the public/index.html

How to remove Turbolinks:

1. Remove the gem 'turbolinks' line from your Gemfile.
2. Remove the //= require turbolinks from your app/assets/javascripts/application.js.
3. Remove the two "data-turbolinks-track" => true hash key/value pairs from your app/views/layouts/application.html.erb.

#### 3) git init

This command sets up your local git repository. You’ll need to do this command first in order to commit your changes later.

#### 4) edit config/database.yml

    host: 127.0.0.1

This file holds some settings for your postgresql database. You will probably want to remove the username on your databases. For my system I also needed to add the line host: 127.0.0.1 to make postgresql work.


#### 5) Gemfile updates

These are some common gems I’ve used:

    Gemfile
    
    gem ‘twitter-bootstrap-rails’
    gem ‘simple-form’

       group :test, :development do
           gem 'rspec-rails'
           gem 'capybara'
           gem 'launchy'
       end

       group :test do
           gem 'factory_girl_rails'
         gem ‘shoulda-matchers’
       end

       group :development do    #access at rails/routes
           gem 'sextant'
       end

       

#### 6) rake db:create

I usually do this before bundle, as I’ve run into some errors otherwise. This command creates a local instance of the postgresql database.

#### 7) bundle

    Install those gems!

#### 8) git add * ; git commit -m ‘message’

A good chance to commit those changes! <code>git add *</code> will add all changed files to your commit. When you do <code>git commit -m ‘message’</code>, those changes are then saved to your local git repository. This is useful if you ever need to go back to a stable version of your app. This command will also be very important to Git collaboration.

#### 9) rails g rspec:install (rails g bootstrap:install static)

Install any gem generated scaffolds you may be using. Rspec is a must! Other examples could include bootstrap, or devise.   

#### 10) remove ~/test

Remove that ~/test dir once you have Rspec setup. A clean app is a happy app.    

#### 11) add to spec_helper.rb

Add this helpful lines to make sure everything works! I like to include the FactoryGirl line as well. That way I don’t have to type <code>FactoryGirl.build</code>, but rather just: <code>build</code>.

       spec_helper.rb
       require 'capybara/rails'
       require 'capybara/rspec'

       config.include FactoryGirl::Syntax::Methods

#### 12) create factories.rb file in spec/support

You’ll need to create the spec/support directory, where you’ll place a new file that will create your factories.

#### 13) git add * ; git commit -m ‘message’

Commit!

#### 14) create models / scaffold / migrations

Time to create any generated code. If you’re not using generated code, you can mostly skip to step 16.

#### 15) git add * ; git commit -m ‘message’

Commit before migrating: JUST IN CASE!

#### 16) rake db:migrate ; rake db:rollback ; rake db:migrate

Use this command to double check that your tables / migrations are correctly configured. This is a best practices sort of deal.

#### 16.5) rake db:test:prepare

This command generates the test environment that Rspec uses. You’ll need to redo this command if you make any new migrations.

#### 17) git add * ; git commit -m ‘message’

Don’t forget to commit after you Migrate!

#### Extras: add database.yml and secret_token.rb to your .gitginore

This is just a good habit to get into. When others fork your repos, having special database settings can give them a headache
when trying to check out your code. Do them a favor and tell git to ignore that file. Another file to add is the secret_token.rb
-- it's called that for a reason! It's supposed to be secret! Protect your users by hiding this file.

In your .gitignore file add the lines:
    
    /config/database.yml
    /config/secret_token.rb

===

### From here you can do whatever you’d like, and you shouldn’t break your app too much. Have fun!



