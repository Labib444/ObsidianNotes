
### Will Learn

- How to register services
- How to resolve services
- Extend and replace the built-in container

### Part 1

DI is a design pattern. Use DI injections. 

Tight coupling is considered an anti-pattern in software development. Because the code becomes harder to maintain. 

```C#
//here using the new keyword introduces tight coupling
var someObject = new Object(); 
```

###### Inversion of Control

- Inverting control of dependency creation
- An external component creates dependencies
- Combines with the dependency inversion principle to achieve loose coupling

### Part 2

### Microsoft Dependency Injection Container

We have register our dependencies to IServiceCollection else it will not be injected into the contructor at runtime. Register all required services (dependencies) with IServiceCollection to avoid runtime exceptions.
![[Pasted image 20221228151447.png]]

For ASP.NET 5

![[Pasted image 20221228150949.png]]


![[Pasted image 20221228151614.png]]

```C#
//In: Program.cs
var services = builder.Services;
//Transient lifetime is a safe choice for default behaviour.
services.AddTransient<IWeatherForecaster, RandomWeatherForecaster>();
//Here we can easily change the implementation by changing the RandomWeatherForecaster.
```

Order of declaration is not too important. An exception to this is when intentionally registering multiple implementations of the same abstraction.

### Part 3

Other places we will need to use dependencies. 
- Logging (ASP.NET Hosting framework auto injects it. No need to inject ourselves.)
- Configuring and options
- Application lifetime
- Hosting environment
- Various factories
- Startup filters
- Object pooling
- Routing

###### Advantages of DI
- Promotes loose coupling of components
- Promotes logical abstraction of components
- Supports unit testing

![[Pasted image 20221228152558.png]]

Learn more about the NullLogger over here.

![[Pasted image 20221228152747.png]]