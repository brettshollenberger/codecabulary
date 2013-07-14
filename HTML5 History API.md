#HTML5 History API

URLs users precision control over the resources they choose to load, and allow them to share a certain page with friends, who can expect to see the same page their friend saw. Unique resources living at unique URLs adds value to the web--but if you change the URL, even via script, it triggers a roundtrip to the remote server, and a full page-refresh--not the best thing for latency, and busy users.

The HTML5 History API allows you to download a portion of the page, and to change the URL.

Suppose you have two pages with nearly identical content. If the user navigates to the first page, and then tries to navigate to the second page, you'll want to interrupt their navigation and manually load the portion of the page that needs to change. In the HTML5 History API, you can accomplish this by:

 1.  Load the portion of the page from the second page that differs from the first (probably using XMLHttpRequest). This will require some server-side changes to your web application. You will need to write code to return just the part of the page that actually needs to be loaded, which can be achieved through a hidden URL or query parameter that the end user wouldn't see.
 2.	Swap in the changed content (using innerHTML or other DOM methods). You may also need to reset any event handlers on elements within the swapped-in content.
 3.	Update the browser location bar with the URL of the second page, using a particular method from the HTML5 history API that we'll go over later.