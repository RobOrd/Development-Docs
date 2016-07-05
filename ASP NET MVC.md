
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

### Action Parameters ###
This is a way on interacting with request values. The controller action in this particular example creates and populates the properties of a new Auction object with values taken from the request.
```csharp
public ActionResult Create()
{
	var auction = new Auction() {	
		Title = Request["title"],
		CurrentPrice = Decimal.Parse(Request["currentPrice"]),
		StartTime = DateTime.Parse(Request["startTime"]),
		EndTime = DateTime.Parse(Request["endTime"]),
	};
}
```

Here is the same controller action as before, this time using model-bound method parameters
```csharp
public ActionResult Create(string title, decimal currentPrice,DateTime startTime, DateTime endTime)
{
	var auction = new Auction() {
		Title = title,
		CurrentPrice = currentPrice,
		StartTime = startTime,
		EndTime = endTime,
	};
}
```

Now, instead of retrieving the values from the Request explicitly, the action declares
them as parameters. When the ASP.NET MVC framework executes this method, it
attempts to populate the action’s parameters using the same values from the request
that the previous example showed. Note that—even though we’re not accessing the
Request dictionary directly the parameter names are still very important, because they
still correspond to values from in the Request.

Folowing example binds directly to an Auction instance, this shows the true power of model binding.
```csharp
public ActionResult Create(Auction auction)
{
	// ...
}
```

### Views ###
- Razor View Engine Resources
	- [http://www.codemag.com/article/1103041](http://www.codemag.com/article/1103041)

- **Code nuggets**: Are simple expressions that are evaluated and rendered inline. Its always return markup for the view to render.
```cshtml
Not Logged In: @Html.ActionLink("Login", "Login")
```

- **Code Blocks**: Is a section of the view that contains strictly code rather than a combination of markup and code. Each line of code written un C# must include a semicolon (;) at the end. Code blocks do not render anything to the view. Instead, they allow you write arbitrary code that requieres no return value.
```cshtml
@{
// The title and bids variables are
// available to the entire view
var title = Model.Title;
var bids = Model.Bids;
}
<h1>@title<h1>
<div class="items">
<!-- Loop through the objects in the bids variable -->
@foreach(var bid in bids) {
<!-- The bid variable is only available within the foreach loop -->
<div class="bid">
<span class="bidder">@bid.Username</span>
<span class="amount">@bid.Amount</span>
</div>
}
<!-- This will throw an error: the bid variable does not exist at this scope! -->
<div>Last Bid Amount: @bid.Amount</div>
</div>
```

### Layouts ###
Razor offers the ability to maintain a consistent look and feel throughout your entire website through layouts. With layouts, a single view acts as a template for all other views to use, defining the site-wide page layout and style. 

A layout template typically includes the primary markup (scripts, CSS stylesheets, and structural HTML elements such as navigation and content containers), specifying locations within the markup in which views can define content. Each view in the site then refers to this layout, including only the content within the locations the layout has indicated.

### Partial Views ###
The most common scenario is needing to display the same high-level information in multiple locations in a site, but only on a few specific pages and in different places on each of those pages.

```cshtml
@model Auction
<div class="auction">	
	<a href="@Model.Url">
		<img src="@Model.ImageUrl" />
	</a>
	<h4><a href="@Model.Url">@Model.Title</a></h4>
	<p>Current Price: @Model.CurrentPrice</p>
</div>
```	

To render this snippet as a partial view, simply save it as its own standalone view file (e.g. /Views/Shared/Auction.cshtml) and use one of ASP.NET MVC’s HTML Helpers **—Html.Partial()—** to render the view as part of another view. 
```cshtml
@model IEnumerable<Auction>
<h2>Search Results</h2>
@foreach(var auction in Model) 
{
	@Html.Partial("Auction", auction)
}
```	