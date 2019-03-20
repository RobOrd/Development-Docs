# Software Design Patterns and Practices

### Some of theory
Software components are subject to change depending on the business. Since software is for business and the reverse is not always true. If so, then we have to keep in mind that, when change comes, our application can adapt smoothly or at least by giving only a little trouble.

- **Software design principle**: Principle provides us guideline. Principle says what is right and what is wrong. It doesn’t say us how to solve problem. It just gives some guideline so that we can design good software and avoid bad design. Some principles are DRY, OCP, DIP, etc.
- **Software design pattern**: Pattern is a general reusable solution to a commonly occurring problem within a given context in software design. Some patterns are factory pattern, Decorator pattern etc.

### Service Locator Pattern
The service locator pattern is a design pattern used in software development to encapsulate the processes involved in obtaining a service with a strong abstraction layer. This pattern uses a central registry known as the “service locator” which on request returns the information necessary to perform a certain task.
The ServiceLocator is responsible for returning instances of services when they are requested for by the service consumers or the service clients. 

Service Locator pattern does not describe how to instantiate the services. It describes a way to register services and locate them. The goal of this pattern is to improve the modularity of your application by removing the dependency between the client and the implementation of an interface.

The service locator pattern is still a good option to remove the dependency between the client and the implementation of an interface when you can't use Dependency Injection. Both patterns have the same goal, but use very different approaches to achieve them. The three most **common arguments against the service locator pattern** are:

- All components need to have a reference to the service locator, which is a singleton.
    - If you use your components in different applications and environments, introducing a dependency to your service locator class might be problematic because the class might not exist in all environments.
    - Implementing the service locator as a singleton can also create scalability problems in highly concurrent environments.
- The service locator makes the application hard to test.
- Higher risk to introduce breaking changes.


##### Basic Sample
The service locator is a singleton. The CoffeeServiceLocator class has a private constructor and keeps a reference to itself. You can get a CoffeeServiceLocator instance by calling the static getInstance method on the CoffeeServiceLocator class.

```csharp
public class CoffeeServiceLocator {
    private static CoffeeServiceLocator locator;

    private CoffeeMachine coffeeMachine;

    private CoffeeServiceLocator() {
        // configure and instantiate a CoffeeMachine
        var beans = new HashMap<CoffeeSelection, CoffeeBean>();
        beans.put(CoffeeSelection.ESPRESSO, new CoffeeBean("My favorite espresso bean", 1000));
        beans.put(CoffeeSelection.FILTER_COFFEE, new CoffeeBean("My favorite filter coffee bean", 1000));
        
        coffeeMachine = new PremiumCoffeeMachine(beans);
    }

    public static CoffeeServiceLocator getInstance() {
        if (locator == null) {
            locator = new CoffeeServiceLocator();
        }
        return locator;
    }

    public CoffeeMachine coffeeMachine() {
        return coffeeMachine;
    }
}

public class CoffeeApp {
    public Coffee prepareCoffee(CoffeeSelection selection)
        var coffeeMachine = CoffeeServiceLocator.getInstance().coffeeMachine();
        var coffee = coffeeMachine.brewFilterCoffee();
        
        return coffee;
    }
}
```

### Dependency Injection Pattern
Dependency injection is a programming technique that makes a class independent of its dependencies. It achieves that by decoupling the usage of an object from its creation. This helps you to follow SOLID’s dependency inversion and single responsibility principles.


---

##### References
- https://stackify.com/service-locator-pattern/
- https://stackify.com/dependency-injection/

https://www.tutorialspoint.com/design_pattern/service_locator_pattern.htm
https://blog.ploeh.dk/2010/02/03/ServiceLocatorisanAnti-Pattern/
http://gameprogrammingpatterns.com/service-locator.html
https://dzone.com/articles/design-patterns-explained-service-locator-pattern
https://www.c-sharpcorner.com/UploadFile/dacca2/service-locator-design-pattern/
