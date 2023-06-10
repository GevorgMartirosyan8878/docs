# RestAPI

Rest is a architectural style or we can say also pattern for APIs

Before we dive deep and understand what makes our API restfull, let's look 
at the two things.

1. **Client** - the client is the software or person whi uses API
    As a developer we can use Twitter API to read and write data from Twitter
    or create new twit and do some actions in program wrote by us. Our program will 
    call Twitter API. Also client can be a browser, 
    when we opening twitter on browser, browser is the client who calls Twitter API
    and uses returned data to render information on web page
2. **Recourse** - a rescourse can be any object, about what the API can provide
    information. In Instagram's API recourse can be a user, a photo, a hashtag.
    Each recourse have uniq identifier and the identifier can be a name or a number
    
The REST set of constraints will make our APIs easier to use and discover

REST stands for REpresentational State Transfer

When a Restful API is called, the server will transfer to the client, 
representation of the state for requested recourse

For example when we call Instagram API, to fetch specific recourse (for example user),
the API will return the state of that user, included their name, the number of posts, 
how many followers have the user and so on.

The representation of the state can be on JSON, XML and HTML formats,
but for most APIs it is JSON.

The client need to provide two thing when calling to API

1. An identifier for the recourse, it was URL or also knows as an endpoint
2. The operation you want server to do. Most common operations was GET, POST, PUT, DELETE

In order for an API to be RESTful, it must follow principles

1. Uniform interface
2. Client - server separation
3. Stateless
4. Layered system
5. Cacheable
6. Code-on-demand

**Uniform interface**:</br>
This constraint has 4 parts</br>
1. The request to the server eed to include recourse identifier
2. Servers response including enough information that client can modify recourse
3. Each request to the API contains all the information that server needs to
   perform the request, and the same time server should response with the all 
   information that client needs.
4. Hypermedia s the engine of application state - if we want to understand simply
   we can think about this point like the server returns a html form from the app
   which include all necessary links or tags (hypermedia) which will show how user
   can interact with API's.

Conclusion for uniform interface is that requests for different clients (
browser, linux server, android application) looks like same.


**Client - server separation**:</br>
The client and server lives independently, and only way to interact between them
is the requests which initialized by clients only, and responses which is 
send to the client only as a reaction to the client request. Server just the site 
which waiting for client requests. The server does not start sending information 
about state of some recourses on its own

**Layered system**:</br>
Between client who request representation for recourse state and the server
who send the response back, there can be a number of servers in the middle,
which can provide for example security layer, caching layer, load-balancing layer
or some other functionality. Those layers must not affect on response or request.

**Cacheable**:</br>
This means that data which sent by server, should contain information that is
allowed to cache it or not. If data is cacheable it should contain some kind
of version number. The version number is what makes caching possible:
since the client know which version data it already has, the client can avoid 
for requesting the same data every time. The client also should know if the current 
version of data expired, in which case client should send a request to get most
updated data about state of recourse

**Code on demand**:</br>
This constraint is optional and means that client can request the code from server
and server send response as a code(for example html) and after it client will 
execute the code in client side

