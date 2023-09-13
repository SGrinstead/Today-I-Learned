### Dependency Injection in an ASP.NET Core App

Dependency Injection is a design pattern that aims to keep your code loosely coupled and easy to maintain. A dependency is an object that another object relies on. 
If your code has too many dependencies, it can become very difficult to make changes to, as any change to an object may impact multiple other objects that rely on it.
The process of dependency injection involves restructuring your objects so that you can instantiate the dependency object a single time, and then pass it in as an
argument when creating the objects that depend on it. As an example, one of my Message Logger program's controller test classes has these lines:

```
public class MessagesControllerTests : IClassFixture<WebApplicationFactory<Program>>
{
	private readonly WebApplicationFactory<Program> _factory;

	public MessagesControllerTests(WebApplicationFactory<Program> factory)
	{
		_factory = factory;
	}
}
```

This uses dependency injection to pass in the WebApplicationFactory. Without using dependency injection, it would look more like this:

```
public class MessagesControllerTests
{
	//any setup for arguments

	private readonly WebApplicationFactory<Program> _factory = new WebApplicationFactory<Program>(/*arguments*/);
}
```

The problems with this approach are not immediately apparent but consider this: If the WebApplicationFactory has dependencies of its own, each of them must now be set up
inside this test class before it is instantiated. This program also has more test classes. If I were to use this approach, I would have to set up a different
WebApplicationFactory, including its dependencies, in each of those test classes. Instead of making one WebApplicationFactory and passing it into multiple locations,
I would have a separate WebApplicationFactory in each place.

Now let's say you would like to make a change to some information that the WebApplicationFactory needs to instantiate. You would have to make changes in the WebApplicationFactory
class, and then change the information in every class you have set up to use the WebApplicationFactory. This is the result of the tight coupling between classes if you don't use
dependency injection.  	
	
Additional Resources:

[Microsoft Documentation](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)  
[C#Corner](https://www.c-sharpcorner.com/UploadFile/85ed7a/dependency-injection-in-C-Sharp/)  
[Auth0 Blog](https://auth0.com/blog/dependency-injection-in-dotnet-core/)
