

# DDD (Domain Driven Design)

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