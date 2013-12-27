# Struts

* All URLs are GET
* Querystring encoding
* responseFormat = json param

* What headers to use?
 
* No entity body; query-string
* What status codes are likely to be returned? -> Either success or failure; if they're failure; No error codes are used in header; they're used in response body

* Most responses contain a status code, session id, and “data” element. Requests to “get” information
will return one/more elements under “data”. When a command fails, the “data” element will be empty,
and an error message will be displayed.

* No special headers except for session cookie
* Always use POST if we can
* 3 different statuses - success, failure, not logged in