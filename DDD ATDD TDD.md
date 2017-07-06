

# DDD (Domain Driven Design)
Domain Driven Design (DDD) is a wonderful technique that attempts to bring our designs closer to the domains we are working on. 

The theory being that the closer we are to the domain in our implementations, the better our software will be. With this goal in mind, DDD has been extremely successful throughout our industry.

Es un conjunto de prácticas, técnicas, herramientas y enfoques para dar respuesta a las necesidades complejas en el desarrollo de software, orientadas al core del negocio, su evolución y sus objetivos.

**Aggregates**: Boundary around objects inside
- can't access objects directly, must go through the Root. 
- Contains no direct references to other Aggregates, only IDs.
		
    ```csharp
	public class Product //Agregate root
	{
		public int Id { get; private set; }
		public int CatalodId { get; private set; }
		public Money Price { get; private set; }

		public void ChangePrice(Money newPrice)
		{
		}
	}
	```

**Factories**: For creation and initialization of complex objects structures. 
- Creates entire Aggregates

**Repositories**: 
- Don't use generic Repository<T>!
- Don't leak DAL concerns in the domain
- Most DAL technology already offer a generic abstraction

    ```csharp
	public interface IProductRepository
	{
		Product LoadProductAggregateById(int id);
		void AddNewProduct(Product product);
      
        //Queries
		IEnumerable<Product> QueryProductsByCatalog(string catalogName);
        IEnumerable<Product> QueryDiscountedProducts(string nameFilter Money maxPriceFilter);
	}
	```

**Entity**: An entity will always have a unique identifier. An entity may have other attributes that define its characteristics. An entity encapsulates state that can change continuos over time, but is the same object with the same identity.

**Value Object**: An object that represents some descriptive aspect of the domain, but has no conceptual identity is called Value Object. A value object is just a value, it quantifies or describes a property of another object, usually an entity. If the property changes the value object can be completely replaced by another with the new value.

***Combinable***
Values are often represented numerically, so in a lot of cases, they can be combined to create a new
value. Combinability like this is a defining characteristic of value objects in general. So when you are with
domain experts and they talk about combining two instances of a certain concept, this is a clear
sign that you may need to model the concept as a value object.

Remember, though, that value objects are immutable, and the result will be a new value object with a desired value. The original objects remain unchanged. Through representing a value or quantity, in most cases value objects can be combined using
operations like addition, subtraction, and multiplication.

**Bounded Context**: One could theoretically conceive of a domain model for the entire enterprise, however such a large domain model is neither feasible nor cost effective. 
	- We must divide large domains into smaller manageable models, each of which is bounded within a context.
	
![](http://i.imgur.com/5n5r6vf.png)

**Patrones de Integracion entre Bounded Contexts**
- Open Host Service(OHS): necesita un lenguaje de publicación común de interacción entre los modelos de dominio. Por ejemplo RPC o REST mediante API. Aunque puede implementarse mediante un middleware de mensajería como RabbitMQ.
- Anticorruption Layer (ACL): Capa aislada que gestiona el mapeo o traducciones entre los modelos de dominio de los Bounded Contexts para mantener la compatibilidad entre ellos. 
	- En dicha capa se implementan las interfaces expuestas como servicios de dominio. 
	![](http://i.imgur.com/sQFvgpj.png)
	

**Event**: Bounded Context generates events to inform that something happened inside it.

***Domain Event Handlers***
What does handler have access to? All of the infrastructure layer interfaces. This makes it a great place to send out emails and notifications, synchronize with third party services, create audit records, etc.
- Domain event handlers should not change the state of the object that caused domain event to be triggered.
- Domain events should be raised inside the domain.
- Domain events handlers should be looked at as a side effect / chain reaction facilitator.

**Layered Architecture**: The basic idea is to structure an application into four conceptual layers.
    - Infrastructure Layer: Capa que implementa/provee todos los servicios de bajo nivel (persistence, sending message, logging), que depende de las abstracciones o interfaces definidas/ubicadas en los componentes de alto nivel.
    - User Interface Layer: Capa de presentación gráfica al usuario. No contiene lógica de negocio siendo el cliente directo de la capa de aplicación.
    - Application Layer: Lógica de aplicación (no de negocio) mediante servicios (Application Services). Cliente directo de la capa de dominio. Coordina las acciones entre los objetos del modelo del dominio.
    - Domain Layer: Contiene la **lógica de negocio** mediante servicios (Domain Services) y **modelado de dominio** (Domain Model). Alojando las abstracciones o interfaces necesarias de acceso a datos (respositorios) y publicación de eventos de dominio (Domain Events).
	
**Onion Architecture**
La principal ventaja o diferencia con la arquitectura por capas habitual se encuentra en desacoplar/extraer la base de datos tratándola como un recurso externo. Siendo encargada la capa de infrastructura en implementar las interfaces de acceso a datos ubicadas en el dominio mediante Repository Pattern. 
![](http://i.imgur.com/lDCLhEK.png)

- **IEventDispatcher**: The IEventDispatcher is actually going to be the IoC container such as Ninject or CastleWindsor etc. It is responsible for finding the right handlers to deal with the EndOfSurvey event. There can potentially be more than one and it will find them all.

- **Raise\<T\>**: The generic method Raise<T> will allow us to create any number of events and the Dispatcher will sort out the handlers later automatically. This gives us a lot of room for extensibility when a business rule changes and our Survey domain object needs to raise a new type of event e.g. SurveyStarted, UserTimedOut etc.

-  **Domain Event Handlers**: Is a project which clearly separates out the different event handlers into their own classes. Each time there's a new handler required, I just simply add to this project. When you create some class like EndOfSurveyHandler.cs we can even use dependency injection in the constructor here if we wanted to for storing things in a repository or any other services required. Clean and separated from the domain object assembly. 
 
	```csharp
	public class StockedHandler : IDomainHandler<Stocked>
	{
		IPurchaseOrderRepository repository;	
    	public void Handle(Stocked @event)
    	{
	           repositoty.Save(new OrderStatus(@event.Status));
    	}
	}
	```

https://lostechies.com/jimmybogard/2009/09/03/ddd-repository-implementation-patterns/
