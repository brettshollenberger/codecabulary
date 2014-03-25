# Localhost

By default on a Unix machine, localhost will route to `127.0.0.1`, the loopback interface for the computer (http://en.wikipedia.org/wiki/127.0.0.1).

The name `localhost` is nothing special, just a configuration set in the file `/etc/hosts` on your machine. Running `cat /etc/hosts` on a Mac with the default settings should produce:

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	      localhost
...
```

Any other names could be mapped in a similar fashion. `127.0.0.1` is a reserved IP address, which always refers to the loopback interface to your machine, but when your computer acts as a server, it could also serve files or websites from other addresses. `0.0.0.0` is another such reserved address, which instead denotes "all interfaces" on your machine, including `127.0.0.1`. For example, the Ruby on Rails framework listens on `0.0.0.0:3000` by default, signaling that any incoming requests to your computer will be routed to the Rails application. Visiting `0.0.0.0:3000`, `127.0.0.1:3000`, or `localhost:3000` will all route to the Rails application (and each will also route to 127.0.0.1:3000 by default). 

