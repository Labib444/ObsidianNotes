
### What will learn

- How ASP.NET Core uses the container
- What to register with DI container
- Service lifetimes

### Part 1

One API request generates one scope. Firstly Client sends a request in the Kestrel web server port which then generates an HttpContext then the Root container generates a scoped container for that request.
![[Pasted image 20221228153051.png]]
![[Pasted image 20221228153214.png]]

### Part 2

### Part 3

- Identify dependencies 
- Locate 'new keyword' usage (Is the object a dependency such the object returns important data.)
	- Are methods called on the type which are required for the consuming type to function
- Apply dependency inversion
	- Accept the dependency via the constructor
- Register the services with container
- Rinse and repeat 

###### Poco Types (Plain old CLR objects)

These are like DTO, ViewModel types. Used only to transfer data and doesn't contain any logic. These are not dependencies and "new" keyword can be used. 

###### Does "new" keyword effect in the test?

Yes? then it is a dependency else Not!

###### Primitive Types and Strings

- Should not be injected or registered in a dependency injection container.
- Value types (structs) cannot be registered with container. (strings can be registered as it is a reference type but donot use it.)
- A common requirement is to provide configuration.
- Prefer the strongly-typed options pattern

### Part 4 (Adding configurations data)

```C#
//In appsettings.json
"Features": {
	"EnableWeatherForecast": false
}
```

```C#
//Create new class
public class FeaturesConfiguration
{
	public bool EnableWeatherForecast {get; set;}
}
```

```C#
//In Program.cs
services.Configure<FeaturesConfiguration>(builder.Configuration.GetSection("Features"));
```

```C#
//In Program.cs
using Microsoft.Extensions.Options;

private readonly FeaturesConfiguration _config;
public pageModel(IOptionsSnapshot<FeaturesConfiguration> options)
{
	_config = options.Value;
}

public showData(){
	var config_data = _config.EnableWeatherForecast;
}
```

###### For Testing

![[Pasted image 20221228190345.png]]


### Part 5

![[Pasted image 20221228191156.png]]

- The dependency injection container tracks the instances in creates
- Objects are disposed of or released for garbage collection once their lifetime ends
- The lifetime affects the creation and reuse of service instances.

### Part 6

###### LifeCycles of Service
- Transient
- Singleton
- Scoped

###### Transient 

A new instance every time the service is resolved. Each dependent class receives its own unique instance when the dependency is injected by the container.

![[Pasted image 20221228201757.png]]

- May contain mutable state
	- No requirement to be thread safe
- Small performance cost
	- Multiple objects are created
	- More work for the garbage collector
- Easiest to reason about
- Therefore, Safest default choice

```C#
builder.Services.AddTrasient<IGuildService, GuildService>();
```

###### Singleton

One shared instance for the lifetime of the container (application).

![[Pasted image 20221228202013.png]]

- If this service is used many times througout the application.
- Generally more performant
	- Allocates less objects
	- Reduces load on GC
- Suited to types with expensive or time cosuming work at creation.
- Must be thread-safe
	- Avoid mutable state (else need to use locking strategies.)
- Suited to: 
	- Functional stateless services
	- Caches (memory cache)
- Consider frequency of use vs. memory consumption
- Possible to create memory leaks using the singleton lifetime
- Beware of singleton services where memory usage grows significantly over time.
- If a service is used very infrequently, the singleton lifetime may not be appropriate.

```C#
builder.Services.AddSingleton<IGuildService, GuildService>();
```

###### Scoped

An instance per scope(request). 

![[Pasted image 20221228202047.png]]

- The container creates a new instance per request
	- Not requred to be thread-safe
- Components used in the request lifecycle receive the same dependency instance.
- Useful if a service may be required by multiple consumers per request.
	- Example "DbContext"
-  Any service that depend on a scoped service should declare itself as scoped or transient but not singleton else it might get null. (Should not be captured by singleton services.)

```C#
builder.Services.AddScoped<IGuildService, GuildService>();
```

### Part 7

- Ensure that the service lifetime is appropriate
	- Consider the lifetime of dependencies
- Captive dependices
	- May live for longer than intended

![[Pasted image 20221228203324.png]]

### Part 8

###### Scope Validation

![[Pasted image 20221228220516.png]]

Turned on in "Development Mode" but not in "Production".
Go to -> Properties -> launchSettings.json

```C#
"profiles": 
{
	"ASPNETCORE_ENVIRONMENT": "Production"
}
```

```C#
//In Program.cs (Donot use this if not needed!)
//If this is written then no InvalidOperationException will show in
//development.
builder.Host.UseDefaultServiceProvider(
	options => options.ValidateScopes = false
)
```

### Part 9

![[Pasted image 20221228222357.png]]

