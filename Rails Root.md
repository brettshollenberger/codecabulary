# Rails Root

Rails root is a silly name. It seems to imply that it refers to the location of the installation of Rails on your system--but that's not what the Rails root is.

The Rails root is like the home directory of each Rails app. If on your computer, you build your app in usr/my_app, then usr/my_app is the Rails root (for that particular app). 

When you run a [Rails new command](http://google.com) (`rails new my_app`), you create the Rails root. The directory my_app (the Rails root) is built in whatever directory you're in when you run the Rails new command. 