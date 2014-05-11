Chameleon Template
==================

This is the template Chameleon app. When starting a new
Chameleon project, ask Ops to fork this repo to a new one
on GitHub and then work from there.


##Installation

First install Node.js v0.10.x at http://nodejs.org

```
$ npm install
$ npm install -g bower grunt-cli
# If you get an access error from the above command, run it with sudo:
$ sudo npm install -g bower grunt-cli
$ bower install
$ grunt server
```

##Config Files

Config files are located in the `configs` directory. Unless you specify
the ENV variable before running Chameleon, it will default to
`configs/development.json`. To run with production vars:

```
$ ENV=production grunt server
```

Production settings include the following:

- Auth server is api.edmodo.com
- Image prefix is our CDN URL //assets.edmodo.com/
- API endpoints are api.edmodo.com/v2.4 for mobile and api.edmodo.com/v3 for One Eye

Note: at the time of this writing (Feb 5) One Eye has yet to be released,
which means the production URL (api.edmodo.com/v3) won't work.


##Public directories

In `index.js` we set two public directory locations with Express:

```javascript
app.use(express.static(__dirname + '/public'));
app.use(express.static(__dirname + '/bower_components/edmodo-bootstrap/assets'));
```

##Google Analytics


##Topbar

- The default topbar is the public version of the Edmodo topbar
- We'll have other topbars for other types of accounts


##Homepage

The homepage is a simple landing page without any API routes.


##Sessions

We use the Mozilla client-sessions cookie module (sets and encrypts sessions
on the client).

We also use Express' session management so traditional testing functionality
continues to work.


Qs
==

Do we copy public Chameleon files from their base directory to the template app?
Or simply point to those files in their directory
