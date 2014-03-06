# Collection+JSON

Collection+JSON (`C+J`) is a media type that has two main benefits over ad hoc JSON documents: standardization (to the extent of machine-parseability) and hypermedia controls (which provide a menu of endpoints for clients to interact with). Clients using `C+J` no longer have to write fiat JSON parsers (at least not at the protocol level), and they no longer have to guess at what endpoints they will be able to interact with, or what valid POST, PUT, and PATCH requests to collection endpoints look like; all this data is contained in a collection template. The query templates provided by `C+J` also describe the ways the client may query for members of the collection. 

## Profiles

Collection+JSON documents provide three ways to link to profiles--which can provide application semantics, thus helping to close the semantic gap. 

1) The profile attribute of the media type header:

	application/vnd.collection+json;profile=http://schema.org/Person
	
2) A link header with the link relation "profile":

	Link: <http://example.org/profiles/vcard>;rel="profile"
	
3) As a link element: 

	"links": [{"link": {"href": "http://example.org/profiles/vcard", "rel": "profile"}}]
	

