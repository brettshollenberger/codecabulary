# Single Page Applications

Traditional web applications refresh the page on every HTTP request, resulting in a huge amount of duplication, and increased latency for the end user. A growing trend to improve perceived latency has been to switch to a single page application interface, which, after the initial load, can handle subsequent loading events asynchronously--without a full page reload.

#### SPA Requests

When a user requests a new view in a single-page application, the application requests the new content required to complete the view using an XMLHttpRequest (XHR), typically by communicating with a server-side REST API or endpoint. 