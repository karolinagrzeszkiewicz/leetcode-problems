## REST

[Notes copied from here](https://medium.com/extend/what-is-rest-a-simple-explanation-for-beginners-part-1-introduction-b4a072f8740f)


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
  
  
  
## Testing Web Applications

**Unit Testing** - Making sure individual functions work
**Client Testing** - We will probably want to check not just whether or not specific functions work, but also whether or not individual web pages load as intended. 
**Selenium** - Using Selenium, we’ll be able to define a testing file in Python where we can simulate a user opening a web browser, navigating to our page, and interacting with it. Our main tool when doing this is known as a Web Driver, which will open up a web browser on your computer. 


## CI/CD: Continuous Integration and Continuous Delivery
- A set of software development best practices that dictate how code is written by a team of people, and how that code is later delivered to users of the application. 
**Continuous Integration:** Frequent merges to the main branch and automated unit testing with each merge
- When different team members are working on different features, many compatibility issues can arise when multiple features are combined at the same time. Continuous integration allows teams to tackle small conflicts as they come.
- Because unit tests are run with each Merge, when a test fails it is easier to isolate the part of the code that is causing the problem.

**Continuous Delivery:** Short release schedules, meaning new versions of an application are released frequently.
- Frequently releasing new versions of an application allows developers to isolate problems if they arise after launch.
- Releasing small, incremental changes allows users to slowly get used to new app features rather than being overwhelmed with an entirely different version

**Tools for Continious Integration:** GitHub Actions will allow us to create workflows where we can specify certain actions to be performed every time someone pushes to a git repository. 


## Docker

- Docker is a containerization software, meaning it creates an isolated environment within your computer that can be standardized among many collaborators and the server on which your site is run. 

- A virtual machine is effectively an entire virtual computer with its own operating system, meaning it ends up taking a lot of space wherever it is running. 

- Dockers, on the other hand, work by setting up a container within an existing computer, therefore taking up less space.

- A Docker image is a read-only template that contains a set of instructions for creating a container that can run on the Docker platform.

- Docker Compose allows two different servers to run in separate containers, but also be able to communicate with one another.


## Scalability

- we run our sites on servers, which are physical pieces of hardware dedicated to running applications. 
- Servers can either be on-premise (We own and maintain physical servers where our application is hosted) or on the cloud (servers are owned by a different company such as Amazon or Google, and we pay to rent server space where our application is hosted). 
- A single server can handle only so many requests at once, forcing us to make plans about what to do when our one server is overworked.
- we have to determine how many requests a server can handle without crashing, which can be done using any number of benchmarking tools including Apache Bench.

## Scaling
- can be done vertically(buy a more powerful server) or horizontally(run servers in parallel)

## Load balancing
- with horizontal scaling, we need to balance how many requests each server gets
- we can employ a load balancer, which is another piece of hardware that intercepts incoming requests, and then assigns those requests to one of our servers. 

- **Problems of Sessions** - 
- we might have sessions that are stored on one server but not another, and we don’t want users to have to re-enter information just because the load balancer pushes their request to a new server
