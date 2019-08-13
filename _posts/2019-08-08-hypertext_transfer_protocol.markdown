---
layout: post
title:      "HyperText Transfer Protocol"
date:       2019-08-08 15:09:31 -0400
permalink:  hypertext_transfer_protocol
---


HTTP is a simple, yet powerful network protocol that is the backbone of communication between clients and servers on the internet. Knowing HTTP enables you to interact with Web servers, pass queries, resources (ie: images), as well as files. It is language independent, however for the purposes of this blog post I’ll be talking about HTTP as it applies to Sinatra and Ruby. Simply stated, HTTP is the rules for asking and sending other computers information.


HyperText – HMTL/XML (Markup)

Transfer – exchange of information from computer to web server

Protocol – formatting rules for this information

	
This protocol is composed of messages that are sent from client to server and back in a cyclical process known as the **Request Response Loop**. Although this cycle can be quite complex as a message finds it’s was from the client to the server and back, we will only be looking at the origin and end result; the *Request* and the *Response*. 

**REQUEST**
HTTP client opens a connection and sends a request message composed of the following parts to an HTTP server:

*	**Request-Line:** Request-Method*, Request-URI*, Protocol Version*
	
*	**Request Header:** request modifiers that allow the client to pass additional information such as host, encoding, cookies etc.

*	**Request Body:** (optional)
	

**RESPONSE:**
HTTP server returns a response message, containing the following message parts and requested resource or error code

* **Message Status-Line**  : Protocol Version*, Status Code*, Reason-Phrase*

* **Response Header** : information about the server and about further access to the resource identified by the Request-URI

* **Response Body** : (optional) A single string containing markup or other information
After delivering this response, the server closes the connection.


*Request-URI* – (Uniform Resource Identifier) resource upon which to apply the request
*Request-Method* – tells the server what you would like to do with the URI (aka HTTP Verb)
*Protocol Version* – uses standard versioning to indicate which version of the protocol is being used. 1.1 includes implementation of features such as caching, persistant connections, etc.
*Status code* – three digit code that states details of the success or failure of a request
*Reason Phrase* – optional textual phrase associated with the status code for understanding
	
HTTP is a stateless protocol, where the server and client are only aware of eachother during a request. Once the server closes the connection, neither party retains this information – both parties essentially forget about each other.

Before we get into examining how Sinatra handles HTTP requests, we need to take a closer look at the various HTTP status codes. Generally speaking, they can be divided into 5 categories:

* **1xx**: *Informational* - request received but is continuing process
* **2xx**: *Success*       - successfully received, understood, and accepted by the server
* **3xx**: *Redirection*   - further action must be taken in order to complete the request
* **4xx**: *Client Error*  - request contains bad syntax or cannot be fulfilled
* **5xx**: *Server Error*  - server failed to fulfill valid request


There are about 50 different possible HTTP Status codes but these are some of the most frequently seen:
Code    Reason Phrase        Description
---------------------------------------------------------------
**200**   *OK*                -   Everything worked out. Server finds the page and returns it to your computer along with this status

**301**   *Moved Permanently* -   Returned by the server when the requested page has moved somewhere else

**303**   *See Other*         -   Returned by the server in response to a POST (or PUT/DELETE). Server has received the data.

**400**   *Bad Request*     -   Server equivalent of WTF. Returned by the server when it can’t understand what you’re asking for.

**403**  * Forbidden*        -   Typically only seen if you’re trying to break some rules. Tisk Tisk.

**404**   *Not Found*        -   Returned by the server when it can’t find the page that you’re  looking for

**500**   *Internal Server Error*-  Generic error message, given when an unexpected condition was encountered and no more specific message is suitable. Equivalent of a Server crying out for help because something’s broken.

**418**  *I’m a Teapot*     -   Put together as an April Fools Day joke in 1998, as part of the specification for the Hyper Text Coffee Pot Control Protocol – meaning status codes for your teapot!

HTTP is lovely and all, how do we apply all of this to web development in Ruby? There are a couple of layers.

First we need to understand Rack. Rack is a minimal web server interface specifically for Ruby and Ruby frameworks that does the heavy lifting. It wraps HTTP requests to standardize communication between web apps and servers.

Sinatra is a lightweight layer that sits on top of Rack. It abstracts these HTTP requests into a handful of methods (defined in it’s base class) that allow developers to focus directly on HTTP requests without worrying about the underlying plumbing. Sinatra is very lean and stays out of the way by only responding to what you tell it to.

For most development purposes, we can focus on five common HTTP Request-Methods (verbs):

* **GET**        - Requests a resource from the server (page, image, stylesheet, etc.)

* **POST**     - Submits data to a web server

* **PUT**        - Create or Replace a single resource on the server.

* **PATCH**   - Update a resource on a server (in contrast to PUT which replaces it whole)

* **DELETE** - Used to destroy a resource on a server

These verbs are built into Sinatra methods called Routes that handle each type of HTTP Request. They are similar to Ruby methods, but have a slightly different syntax:

Route block containing logic/templates for that page:

   **http_verb** '/:path' do
      #template info
      erb: 'template.rb'     
    end

Assuming that you have a very basic understanding of MVC architecture, lets visually step through through the flow of a **GET** request in a basic Sinatra app:

User                 Browser                  App                      View                  Data
![get_request](https://photos.google.com/photo/AF1QipPxtCei_WLZlbOKEaZFo9OdHtRkrQsFapFLYQym)

￼
Building on top of that, let’s look at the **DELETE** method:

		**delete** '/:resource' do
			*#image, page, or other resource location*
			redirect: 'hello_world.rb'
		end
		
This builds upon the *GET* request, as specified in version 1.1 of the HTTP specification. The connection remains open until the entire request has finished, in this case it’s a delete request stacked on top of redirect that then fetches the HTML for a default page:

User                 Browser                  App                      View                  Data
￼![delete request](https://photos.google.com/photo/AF1QipNWCWUUpnuqgb3eSXdrW-vV3RhfX278AHbtU1Lj)

The next request in complexity is the **POST** method:

		**post ** '/:path' do
			*#html template or resource to be created on the server*
		end
		
Similar in behavior, but not to be confused with the  *PATCH* (update) methods, a **POST** request creates a brand new resource on the server at the specified path:

User                 Browser                  App                      View                  Data

![post](https://photos.google.com/photo/AF1QipMpTuU5gGr8UmH7K23ia7405xbOjIbr2NX6MUYl)

￼
Last but not least, lets have a look at the **PUT** and **PATCH** HTTP requests. Sandwiched in between two get responses, these request result in an updated or entirely new resource at the specified path:

		**put**  '/:path' do
			*#html template or resource to be replaced on the server*
		end

		**patch**  '/:path' do
			*#html template or resource to be updated on the server*
		end

These two are so similar in nature that the difference in what is actually being done on the server is negligible. The difference in the verbs is more for developer understanding:

User                 Browser                  App                      View                  Data
￼![put/[patch](https://photos.google.com/photo/AF1QipM05yqzp6pso9yk5o4mAYuMWlGCj4vbXSngONIe)

 
There is, of corse, more to HTTP than just the basics I’ve run through here. These are the the most common interactions that you will have with HTTP when building for Sinatra and this brief foundation will get you a long way. For further information or to just satisfy your general curiosity feel free to explore the links below.

##### Resources
* http://www.jmarshall.com/easy/http/#whatis 

*  http://en.wikipedia.org/wiki/List_of_HTTP_status_codes 

*  http://shop.oreilly.com/product/0636920019664.do 

*  http://www.tutorialspoint.com/http/http_tutorial.pdf   

*  http://www.w3.org/Protocols/rfc2616/rfc2616.html 

*  http://www.sinatrarb.com http://skillcrush.com/2012/05/11/404-error/
