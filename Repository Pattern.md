Repository Pattern Solve the DDD’s One Repository Per Aggregate Root

In Domain Driven Design there is a concept called aggregate root. The aggregate root is the entity that act as a parent or root for other set of related entities. So these associated entities only make sense if they are attached to the aggregate root. Imagine we have a table called Book and another table called BookReview. The existence of BookReview table does not make sense without the Book table. What repository pattern supposedly solve is that we can gather all of the database logic related to the whole aggregate into one repository.

So any operation on a BookReview should be done through a Book. Since the existence of BookReview is entirely dependent on a Book. But when we use an ORM, this problem simply doesn’t exist. We can navigate and change an entity which is a root and all the entities that are connected to that entity. We do that simply by using navigation properties in most ORMs.

- https://lostechies.com/jimmybogard/2012/10/08/favor-query-objects-over-repositories/
- https://lostechies.com/jimmybogard/2009/09/03/ddd-repository-implementation-patterns/
- https://deviq.com/repository-pattern/
- http://hamidmosalla.com/2018/11/25/stop-using-repository-pattern-with-an-orm/
- https://github.com/HamidMosalla/FreelancerBlog
- https://www.stevejgordon.co.uk/cqrs-using-mediatr-asp-net-core
- https://blogs.msdn.microsoft.com/cdndevs/2016/01/26/simplifying-development-and-separating-concerns-with-mediatr/