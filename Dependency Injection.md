# Dependency Injection

### Concepts

#### Lifecycle
When we register our dependencies, we can choose from three different lifecycle settings.
- **Transient**: A new instance of the dependency is going to be created upon every retrieval.
- **Scoped**: One instance of the dependency is going to be used per scope. In ASP.NET this means that one instance is going to be created per HTTP request. This comes handy if our class depends on some property of the HttpContext.
- **Singleton**: Only one single instance of the dependency is going to be created and used for all retrievals.