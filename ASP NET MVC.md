
# Architecture #

### The Model ###
- The model represents core business logic and data
- Models encapsulate the properties and behavior of a domain entity and expose properties that describe the entity

### The View ###
- The view is responsible for transforming a model or models into a visual representation
- Views should concentrate only on displaying data
and should not contain any business logic themselves

### The Controller ###
- The controller, as the name implies, controls the application logic and acts as the coordinator between the view and the model
- Controllers receive input from users via the view, then work with the model to perform specific actions, passing the results back to the view.

### What’s New in ASP.NET MVC 4 ###
- Asynchronous controllers
- Display modes
- Bundling and minification
- Web API

ASP.NET MVC relies on the concept of convention over configuration whenever possible.
This means that, instead of relying on explicit configuration settings, ASP.NET
MVC simply assumes that developers will follow certain conventions as they build their applications.

There are three special folders in the project that correspond to the elements of the MVC pattern: the **Controllers**,
**Models**, and **Views** folders.

### Routing ###
All ASP.NET MVC traffic starts out like any other website traffic: with a request to a URL. This means that, despite the fact that it is not mentioned anywhere in the name, the ASP.NET Routing framework is at the core of every ASP.NET MVC request.

ASP.NET routing is just a pattern-matching system.
	
The default ASP.NET MVC project templates add a generic route that uses the following URL convention to break the URL for a given request into three named segments, wrapped with brackets ({}): “controller”, “action”, and “id”:

{controller}/{action}/{id}

When the application starts up, ASP.NET MVC discovers all of the application’s controllers by searching through the available assemblies for classes that implement the **System.Web.Mvc.IController** interface (or derive from a class that implements this interface, such as **System.Web.Mvc.Controller**) and whose
class names end with the suffix Controller. The controller and action values in a route are not case-sensitive.

### Controllers ###
In the context of the MVC architectural pattern, a controller responds to user input
(e.g., a user clicking a Save button) and collaborates between the model, view, and
(quite often) data access layers. In an ASP.NET MVC application, controllers are classes
that contain methods that are called by the routing framework to process a request.


### Action Results ###

Despite the fact that every controller action needs to return an ActionResult, you will
rarely be creating them manually. Instead, you’ll usually rely on the helper methods
that the System.Web.Mvc.Controller base class provides, such as:
	
	public ActionResult About()
	{
		ViewBag.Message = "Your application description page.";
	
		return View();
	}
	

- **Content()**
Returns a **ContentResult** that renders arbitrary text, e.g., “Hello, world!”
- **File()**
Returns a **FileResult** that renders the contents of a file, e.g., a PDF.
- **HttpNotFound()**
Returns an HttpNotFoundResult that renders a 404 HTTP status code response.
- **JavaScript()**:: Returns a **JavaScriptResult**
that renders JavaScript, e.g., “f	unction hello() { alert(Hello, World!); }”.
- **Json()**
Returns a **JsonResult** that serializes an object and renders it in JavaScript Object
Notation (JSON) format, e.g., “{ “Message”: Hello, World! }”.
- **PartialView()**
Returns a **PartialViewResult** that renders only the content of a view (i.e., a view
without its layout).
- **Redirect()**
Returns a **RedirectResult** that renders a 302 (temporary) status code to redirect
the user to a given URL, e.g., “302 http://www.ebuy.com/auctions/recent”. This
method has a sibling, **RedirectPermanent()**, that also returns a **RedirectResult**, but
uses HTTP status code 301 to indicate a permanent redirect rather than a temporary
one.
- **RedirectToAction()** and **RedirectToRoute()**
Act just like the Redirect() helper, only the framework dynamically determines
the external URL by querying the routing engine. Like the **Redirect()** helper, these
two helpers also have permanent redirect variants:** RedirectToActionPermanent()**
and **RedirectToRoutePermanent()**.
- **View()**
Returns a **ViewResult** that renders a view.