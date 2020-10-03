## REST

y


REST is an atchitectural style for APIs. An API is called RESTful if it uses this architecture.

**Clinet:** The Client uses the REST API. The client can be a web browser or a developer.

**Resourse:** The resource is any object the API provides. For example, it can be a photo or a tweet. Each resource has a unique ID or unique name. 

A RESTful API **exposes information** about itself in the form of resources, **create new resources**(create a user) and **take action on these resources**
(edit a post).

- Following the RESTful requirements makes it easier for the client to **use** and discover your API. 

- REST stands for REpresentational State Transfer.
- It means that when an API is called, the server will transfer to the client a representation of the state of the requested resource.
- The representation of the state can be in a JSON format, HTML or XML


**What does the server do when the client calls an API endpoint:**

1. The client needs to provide an **identifier** for the resource they are interested in. 
The URL is an identifier. The identifier is also known as the endpoint. In fact, URL stands for Uniform Resource Locator.

2. The **operation** you want to perform, the operations are a HTTP method or verb. The common HTTP methods are GET, POST, PUT, and DELETE.

Example: In twitter, if the identifier is "www.twitter.com/jk_rowling", the HTTP method GET indicates that we want to get the state of that user.


**Requrements for your API to be RESTful:**

1. Uniform interface
  - the request to the server has to include a resource identifier
  - the response the server returns include enough information so the client can modify the resource
  - the server can inform the client , in a response, of the ways to change the state of the web application.
  - The result of the uniform interface is that requests from different clients look the same, whether the client is a chrome browser, an android app or anything else.
  
2. Client — server separation
  - The server doesn’t start sending away information about the state of some resources on its own. The client and the server act independently, each on its own.
  
3. Stateless
  - Stateless means the server does not remember anything about the user who uses the API.
  
4. Layered system
  - Between the client who requests a representation of a resource’s state, and the server who sends the response back, there might be a number of servers in the middle. These servers might provide a security layer, a caching layer, a load-balancing layer, or other functionality.
  
5. Cacheable
  - This means that the data the server sends contain information about whether or not the data is cacheable. If the data is cacheable, it might contain some sort of a version number. The version number is what makes caching possible: since the client knows which version of the data it already has (from a previous response), the client can avoid requesting the same data again and again.
  
6. Code-on-demand (optional)
  - The client can request code from the server, and then the response from the server will contain some code, usually in the form of a script,
