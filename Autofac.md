# Autofac
The idea behind inversion of control is that, rather than tie the classes in your application together and let classes “new up” their dependencies, you switch it around so dependencies are instead passed in during class construction.

The basic pattern for integrating Autofac into your application is:
- Structure your app with inversion of control (IoC) in mind.
- Add Autofac references.
- At application startup…
- Create a ContainerBuilder.
- Register components.
- Build the container and store it for later use.
- During application execution…
- Create a lifetime scope from the container.
- Use the lifetime scope to resolve instances of the components.

### Register
##### Use As'<T'> in Delegate Registrations
Autofac infers implementation type from the expressions you use to register components; makes the type **Component** the **LimitType** of the component

```
builder.Register(c => new Component()).As<IComponent>();
```

Works, but avoid this:
```
builder.Register(c => (IComponent)new Component());
builder.Register<IComponent>(c => new Component());
```

##### Use Constructor Injection
The concept of using constructor injection for required dependencies and property injection for optional dependencies is quite well known. An alternative, however, is to use the “Null Object” or “Special Case” pattern to provide default, do-nothing implementations for the optional service

##### Use Relationship Types, Not Service Locators
Giving components access to the container, storing it in a public static property, or making functions like Resolve() available on a global “IoC” class defeats the purpose of using dependency injection. Such designs have more in common with the Service Locator pattern.

### Instance Scope
Instance scope determines how an instance is shared between requests for the same service

##### Instance Per Dependency
Also called **transient** or **factory** in other containers. Using per-dependency scope, a unique instance will be returned from each request for a service.

This is the **default** if no other option is specified.
```csharp
var builder = new ContainerBuilder();

// This...
builder.RegisterType<Worker>();

// ...is the same as this:
builder.RegisterType<Worker>().InstancePerDependency();
```

When you resolve a component that is instance per dependency, you get a new one each time.
```csharp
using(var scope = container.BeginLifetimeScope())
{
  for(var i = 0; i < 100; i++)
  {
    // Every one of the 100 Worker instances
    // resolved in this loop will be brand new.
    var w = scope.Resolve<Worker>();
    w.DoWork();
  }
}
```
