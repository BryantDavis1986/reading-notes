# Readings: DB Seeding and Intro to APIs
## Routing within MVC
* When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing. ASP.NET Routing is setup in two places.
* First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file). There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section. Be careful not to delete these sections because without these sections routing will no longer work.
* Second, and more importantly, a route table is created in the application's Global.asax file. The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events. The route table is created during the Application Start event.
* When an MVC application first starts, the Application_Start() method is called. This method, in turn, calls the RegisterRoutes() method. The RegisterRoutes() method creates the route table.
* The default route table contains a single route (named Default). The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named id.
* Imagine that you enter the following URL into your web browser's address bar:

/Home/Index/3

The Default route maps this URL to the following parameters:

controller = Home

action = Index

id = 3
* When you request the URL /Home/Index/3, the following code is executed:

HomeController.Index(3)
* The Default route includes defaults for all three parameters. If you don't supply a controller, then the controller parameter defaults to the value Home. If you don't supply an action, the action parameter defaults to the value Index. Finally, if you don't supply an id, the id parameter defaults to an empty string.
* The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing. We examined the default route table that you get with a new ASP.NET MVC application. You learned how the default route maps URLs to controller actions.

## Routing within Core
* Routing is responsible for matching incoming HTTP requests and dispatching those requests to the app's executable endpoints. Endpoints are the app's units of executable request-handling code. Endpoints are defined in the app and configured when the app starts. The endpoint matching process can extract values from the request's URL and provide those values for request processing. Using endpoint information from the app, routing is also able to generate URLs that map to endpoints.
* Apps can configure routing using:

Controllers
Razor Pages
SignalR
gRPC Services
Endpoint-enabled middleware such as Health Checks.
Delegates and lambdas registered with routing.
* All ASP.NET Core templates include routing in the generated code. Routing is registered in the middleware pipeline in Startup.Configure.
* Routing uses a pair of middleware, registered by UseRouting and UseEndpoints:

UseRouting adds route matching to the middleware pipeline. This middleware looks at the set of endpoints defined in the app, and selects the best match based on the request.
UseEndpoints adds endpoint execution to the middleware pipeline. It runs the delegate associated with the selected endpoint.
* The preceding example includes a single route to code endpoint using the MapGet method:

When an HTTP GET request is sent to the root URL /:
The request delegate shown executes.
Hello World! is written to the HTTP response. By default, the root URL / is https://localhost:5001/.
If the request method is not GET or the root URL is not /, no route matches and an HTTP 404 is returned.
* The MapGet method is used to define an endpoint. An endpoint is something that can be:

Selected, by matching the URL and HTTP method.
Executed, by running the delegate.
* In the preceding example, there are two endpoints, but only the health check endpoint has an authorization policy attached. If the request matches the health check endpoint, /healthz, an authorization check is performed. This demonstrates that endpoints can have extra data attached to them. This extra data is called endpoint metadata:

The metadata can be processed by routing-aware middleware.
The metadata can be of any .NET type.
* The routing system builds on top of the middleware pipeline by adding the powerful endpoint concept. Endpoints represent units of the app's functionality that are distinct from each other in terms of routing, authorization, and any number of ASP.NET Core's systems.
* An ASP.NET Core endpoint is:

Executable: Has a RequestDelegate.
Extensible: Has a Metadata collection.
Selectable: Optionally, has routing information.
Enumerable: The collection of endpoints can be listed by retrieving the EndpointDataSource from DI.
* 