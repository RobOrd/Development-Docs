# Dependency Injection

### Concept
Dependency Injection (DI) is a pattern that can help developers decouple the different pieces of their applications. It provides a mechanism for the construction of dependency graphs independent of the class definitions.

The Composition Root pattern states that the entire dependency graph should be composed in a single location “as close as possible to the application’s entry point”. This could get pretty messy without the assistance of a framework. DI frameworks provide a mechanism, often referred to as an Inversion of Control (IoC) Container, for offloading the instantiation, injection, and lifetime management of dependencies to the framework. You invert the control of component instantiation from the consumers to the container, hence “Inversion of Control”.

To do this, you simply register services with a container, and then you can load the top level service. The framework will inject all child services for you.

#### Lifecycle
When we register our dependencies, we can choose from three different lifecycle settings.
- **Transient**: A new instance of the dependency is going to be created upon every retrieval.
- **Scoped**: One instance of the dependency is going to be used per scope. In ASP.NET this means that one instance is going to be created per HTTP request. This comes handy if our class depends on some property of the HttpContext.
- **Singleton**: Only one single instance of the dependency is going to be created and used for all retrievals.


### Resources
- http://www.palmmedia.de/blog/2011/8/30/ioc-container-benchmark-performance-comparison
- https://codingsight.com/configuation-comparison-dependency-injection-containers/

- https://simpleinjector.readthedocs.io/en/latest/quickstart.html
- https://jimmybogard.com/automapper-usage-guidelines/