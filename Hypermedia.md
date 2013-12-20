# Hypermedia

One of the challenges of writing web APIs is called the "semantic challenge;" it is the challenge of describing the problem domain so that the client knows what requests the server will respond to. Hypermedia aims to present the client with a menu of options for interacting with the server. It tells the client which HTTP verbs it should use when constructing resources, and which resources are available to interact with.

Although the HTTP protocol can offer machine-predictable ways for requesting new resource generation, updates, deletes, etc, we need hypermedia controls to describe the problem domain. "Hypermedia controls" are the various tools that developers use to describe the requests that can be made of a server. HTML defines several hypermedia controls, like the `<a>` tag, which describes a GET request of a resource.

#### The 3 Jobs of Hypermedia Controls

As evidenced by the `<a>` tag, hypermedia controls have 3 jobs:

1) Tell the client how to construct an HTTP request: describe the HTTP method to use, the URL to use, and the headers and entity body to send.

2) They make promises to the client about the response that is likely to be returned from the server. They suggest the status codes, headers, and type of data the server is likely to respond with. The `<a>` tag is likely to respond with a `200` and the requested page, or a `301` and a redirect to the new location of the page. It's also likely to respond with `404`, resource not found, and other similar error messages.

3) They suggest how the client should incorporate the response into its workflow. In the case of the `<a>` tag, the response will become the new view, leaving the previous page behind, unless the control also requested the new view to open in a new tab, for example.

The goal of hypermedia in RESTful architecture is to allow servers to communicate to clients which URLs the server will respond to; hypermedia presents the client with a menu of options for interacting with the server. It tells the client which semantic subset of the HTTP protocol it should use when requesting resources (GET, POST, HEAD, DELETE, etc), and which resources are available to interact with. Hypermedia can help to solve some of the semantic challenge of creating web APIs--namely that although the HTTP protocol can offer machine-readable predictability in some ways, describing the resources and language of the problem domain is not presently standardized. 

#### Hypermedia Controls

HTML is the most common hypermedia format, and it defines four well-known "hypermedia controls," elements that describe HTTP requests that the server will respond to. 

The `<a>` tag is well known; as a hypermedia control, it acts as a promise _from the server_ that the tag represents a resource you can visit. 

The `<img>` tag is also well-known, and automatically filled for users and embedded in the page.

The `<form>` tag can describe two types of HTTP request (GET and POST). These are the only HTTP verbs implemented by HTML. The values entered on the form's controls are used to generate an HTTP request. 

For a POST:

	POST /form-action HTTP/1.1
	Host: Your Host
	Content-Type: application/x-www-form-urlencoded
	
	message=Hello&submit=Post
	
For a GET:

	GET /form-action/?query=what-i-want HTTP/1.1
	Host: Your Host
	
We notice that POSTed items in HTML generate content-type url-encoded; similarly, params passed via GET are embedded in the query string.

