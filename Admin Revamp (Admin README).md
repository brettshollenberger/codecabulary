Admin Revamp
============

The new Admin site built on [Chameleon](https://github.com/edmodo/chameleon).

##Overview

- [Installation/Deployment](#installation)
- [The Main Layout File](#modifying-the-layout-file)
- [Public and private routes/pages](#making-a-routepage-public)
- [Faking API routes with our API Proxy](#api-proxy)
- [Server-side Plugins](#server-plugins)
- [EventEmitter](#eventemitter)
- [Form Validation](#form-validation)
- [Adding LESS files](#adding-new-less-files)
- [Adding Handlebars helpers](#adding-handlebars-helpers)
- [Sessions](#sessions)

##Installation

- [Prerequisites](#prerequisites)
- [In Development](#in-development)
- [In QA](#in-qa)
- [In Production](#in-production)

###Prerequisites

- [Node.js v0.10.latest](http://nodejs.org/)
- [GraphicsMagick](http://www.graphicsmagick.org/index.html)

On UNIX systems (OS X included) you might get a problem with EMFILE
"too many open files" errors when grunt restarts the process
after save. Just set:

```
$ ulimit -n 2048
```

Should solve the problem.

###In Development

```
$ npm install
$ npm install -g bower grunt-cli supervisor
$ bower install
$ grunt server
```

###In QA

In QA we want the process to fail on its own.

```
$ npm install
$ npm install -g bower grunt-cli supervisor
$ bower install
$ sudo ENV=grunt compile
$ sudo ENV=qa node index.js
```

###In Production

Looks like prod will use daemon to automatically restart the process when it fails.

```
$ npm install
$ npm install -g bower grunt-cli supervisor
$ bower install
$ grunt compile
$ ENV=production node index.js
```

##Modifying the layout file

In `app/templates` is a file named `__layout.default.hbs`. During compilation
and/or when you start the server, this file is copied to `__layout.hbs`. If
you want to modify anything in the layout file, be sure to do it to
`__layout.default.hbs`.

`__layout.hbs` is ignored by git.

**Warning:** don't modify the path of the assets /mergedAssets.js and /styles.css.
If you need to modify these or add new assets, that is done in Gruntfile.js.


##Making a route/page public

In `app/routes.js` we've added a third parameter to the `match` method. If
undefined or false, the route will require the user to have been auth'd to
view it. If `true` then authentication is not required.

ex:

```javascript
// The /funding route requires auth. The user will be routed to the login
// page if they visit this page without the proper credentials
match('/funding', 'funding#index');

// A district page is public, so does not require auth
match('/districts/:district_id', 'districts#show', true);
```


##API Proxy

While routes are being built out on One API, we still need to load data
into our pages to build out the front-end. That's why the
[API proxy](https://github.com/mattpardee/node-api-proxy) was built; it
"catches" routes you specify and returns canned JSON responses. 
Our app doesn't know the difference.

###Adding a route and data

In `tools/api-proxy/config.json` you'll see something like this:

```javascript
{
    "canned": {
        "/v2.4/funding" : "../../tools/api-proxy/canned/funding.json",
        "/v2.4/schools" : "../../tools/api-proxy/canned/schools.json",
        "/v2.4/members" : "../../tools/api-proxy/canned/members.json",
        "/v2.4/districts/:district_id" : "../../tools/api-proxy/canned/district.json"
    },

    "proxies": [
        {
            "host": "https://api.edmodoqa.com/v2.4",
            "proxyURL": "/v2.4/"
        },
        {
            "host": "http://oneapi.edmodoqabranch.com",
            "proxyURL": "/v3/"
        }
    ]
}
```

When adding a new canned response, add it to the "canned" section. Routes
are agnostic -- it uses the same RegEx catcher as Backbone to match
routes like '/v2.4/districts/5' and '/v2.4/districts/234'. The value for
each route points to the file with the JSON data you want to return. See
the `/tools/api-proxy/canned/schools.json` file for how multiple entries
can be returned.

For more info:

- [Node API proxy repo](https://github.com/mattpardee/node-api-proxy)


##Server plugins

We use [Architect](https://github.com/c9/architect) to manage server-side
plugins. The plugin model is pretty simple. Just add a new folder to
`/server/plugins` with two files: the main file, and a package.json file.
package.json describes the plugin and sets a property for the main file to
load for the plugin. For ex:

```javascript
{
    "name": "lb-status",
    "version": "0.0.1",
    "description": "Routes for load balancer status checks",
    "main": "lb-status.js",
    "private": true,

    "plugin": {
        "consumes": ["server"]
    }
}
```

This is a plugin for the Ops load-balancer checks. Its "main" file is
lb-status.js which contains the code. It has a "plugin" property which
contains two possible properties:

- "consumes"
- "provides"

"consumes" is an array of other plugins it will access. In this case, it
needs access to the server so it can add a few routes. This is how lb-status.js
does it:

```javascript
module.exports = function setup(options, imports, register) {
    var app = imports.server.app;

    app.get('/status-lb', function(req, res) {
        res.end('ok');
    });

    app.get('/status-monitor', function(req, res) {
        res.end(JSON.stringify({'status': 'ok'}));
    });

    app.get('/status-human', function(req, res) {
        res.end('hi. everything is still ok. :-)');
    });

    register(null, {});
};
```

Because it wants to consume "server", as laid out in the package.json file,
it can access it from the imports variable passed to it via the setup method.

If it wanted to provide some functionality, it would register that functionality
in the `register` method. `register` is also a variable passed via the
setup method at the top of every plugin. In short, every plugin starts with
this structure:

```javascript
module.exports = function setup(options, imports, register) {
	register(null, {});
};
```

If it wants to register some functionality, it needs to do a couple things

- Update package.json to include its method in the "provides" var
- `register` that method described in the provides var

For ex in server/plugins/server/server.js:

```javascript
var express = require('express'),
	rendr = require('rendr'),
	app = express(),
	sessions = require('client-sessions'),
	appRoutes = require('../../../app/routes.js');

module.exports = function setup(options, imports, register) {

	/**
	 * ...a bunch of app code including that time when it
	 * created the variable app and dataAdapter...
	 */

	register(null, {
		server: {
			app : app,
			dataAdapter : dataAdapter
		}
	});
};
```

And in server/plugins/server/package.json:

```javascript
{
    "name": "server",
    "version": "0.0.1",
    "main": "server.js",
    "private": true,

    "plugin": {
        "provides": ["server"],
        "consumes": ["routeRegex", "adapter"]
    }
}
```

Architect is really just a way to provide and consume other plugins. It's very
simple. Generally, you can wrap your existing server code in a plugin with just
a few lines of code. If other plugins need to access it, then provide a way with
the `register` method. If not, then don't. Just consume other plugins.

To get more information on how architect works and see some more demo
applications, check out its [repo on GitHub](https://github.com/c9/architect).


##EventEmitter

We use a singleton pattern for emitting and listening to events. For an example,
look in [app/app.js](https://github.com/edmodo/admin-pages/blob/master/app/app.js)
for how events are triggered, for ex to hide tooltips:

```javascript
var eee = require('../assets/javascripts/EventEmitter.js');
eee.emitEvent('e.tooltip.hide');
```

In app/events.js we call a `start` method like so:

```javascript
var eee = require('../assets/javascripts/EventEmitter.js');

/**
 * Set up global event listeners
 */
module.exports = {
    start : function() {
        eee.addListener('e.tooltip.hide', function() {
          $('.tooltip').hide();
        });
    }
};
```

This is for global functionality not tied to any single view. For single view
events, use the [Backbone strategy](http://backbonejs.org/#Events).

EventEmitter resources:

- [Guide](https://github.com/Wolfy87/EventEmitter/blob/master/docs/guide.md)
- [API](https://github.com/Wolfy87/EventEmitter/blob/master/docs/api.md)

##Form validation

We are using [bootstrapValidator](https://github.com/nghuuphuoc/bootstrapvalidator).


##Adding new LESS files

Add your new LESS file to `/assets/stylesheets` and then add the filename to
the list of `@import` statements in `/assets/stylesheets/index.less`. e.g. if
you created a new file called "members.less" and you want it to be added, you
would put `@import "members";` at the end of the @import list:

```css
@import "elements";
@import "base_view";
@import "edmodo";
@import "topnav";
@import "login";
@import "groups";
@import "groups-sidebar";
@import "group-header";
@import "message-feed";
@import "profile";
@import "postbox";
@import "funding";
@import "schools";
@import "404";
@import "attachments";
@import "post-reply";
@import "members";
```

##Adding handlebars helpers

Add them to `app/lib/handlebarsHelpers.js`

##Sessions

We use the Mozilla client-sessions cookie module (sets and encrypts sessions
on the client).

We also use Express' session management so traditional testing functionality
continues to work.
