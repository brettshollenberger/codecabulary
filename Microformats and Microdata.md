# Microformats and Microdata

Using hypermedia-based formats can help us in defining our APIs, and the relationships between resources. When hypermedia APIs change, the clients are automatically notified of the changes, and as long as they're still capable of parsing them, they can easily adapt to changes on our server side. 

One distinct advantage is that it has built-in hypermedia controls that are generic enough to be reused in any type of application. Unlike a fiat standard (like an API serving XML that describes games of Battleship), HTML defines hypermedia controls that do very generic things like request outbound resources and either move the current browser window to those resources or embed them within the current document. The challenge then is in conveying our application semantics via HTML. There are two working standards to deal with this: microformats and microdata. 

## Microformats

Microformats are lightweight industry standards developed informally rather than RFCs. Microformats make use of HTML's class attribute to describe application semantics. For instance, provided a specification like hCard, we can describe a "person" in HTML form:

	<div class="vcard">
		<span class="fn">Brett Cassette</span>
		<span class="bday">2050-01-01</span>
		<a href="http://bc.com" class="personalsite" rel="personalsite">BC.com</a>
	</div>

Microformats have the flexibility to describe the semantics of our application, and enable us to define new link relations. Since the class attribute can be used as many times as we like within a document, we can reuse this layout over and over again, and combine documents with similar formats without running into errors. The problem with microformats though is that HTML's `class` attribute was intended to be used for application layout and styling, not describing application semantics. 

## Microdata

Microdata on the other hand is a refinement of the microformat concept for HTML5. HTML Microdata introduces five new attributes specifically for representing application semantics. 

`itemscope`- Creates an Item and indicates that descendants of this element contain information about it
`itemtype` - A valid URL of a vocabulary that describes the item and its properties context
`itemid` - Indicates a unique identifier of the item
`itemprop` - Indicates that its containing tag holds the value of the specified item properties. The properties name and value context are described by the items vocabulary. Properties values usually consist of string values, but can also use URLs using the `a` element and its `href` attribute, the `ing` element and its `src` attribute, or other elements that link to external resources.
`itemref` - Properties that are not descendants of the element with the `itemscope` attribute can be associated with the item using this attribute. Provides a list of element ids (not `itemid`s) with additional properties elsewhere in the document. 
