
- HttpContext
- Forms
- Http Clients

These seem easy to implement but in reality under heavy load these easy implementations may not work.

### Part 1

BaseClasses may have HttpContext already no need for to inject HttpContextAccessor on those pages.

- Improper use of HttpContext can result in hangs, app crashes, or data corruption
- HttpContext: Available in Controllers and PageModels using HttpContext property.
- IHttpContextAccessor for other classes using DI builder.Services.AddHttpContextAccessor()
- HttpContext is not thread-safe
	- Watch out for parallel actions

### Part 2

```C#
//Incorrect Way
public class ApiCaller : IApiCaller
{
	private HttpContext _httpContext;
	pubilc ApiCaller(IHttpContextAccessor httpContextAccessor)
	{
		_httpContext = httpContextAccessor.HttpContext;
	}
}
```

HttpContext maybe null there may throw exceptions.

```C#
//Correct Way
public class ApiCaller : IApiCaller
{
	private readonly IHttpContextAccessor _httpContextAccessor;
	pubilc ApiCaller(IHttpContextAccessor httpContextAccessor)
	{
		_httpContextAccessor = httpContextAccessor;
	}

	public async Task getData()
	{
		if( _httpContextAccessor.HttpContext == null )
		{
			throw new Exception("Can't get access token!");
		}
		var token = await _httpContextAccessor.HttpContext.GetTokenAsync("access_token");
	}
}
```

### Part 3

Avoid running httpContext asynchronously. Rather call the it first then pass down the data to asynchronous functions.

![[Pasted image 20221228224640.png]]

### Part 4

- For forms and body content, use async methods or framework features.  

### Part 5

HttpClient

- First rule: don't create them for each usage! (else may result socket exhausted!)
- Multiple ways to use IHttpClientFactory correctly:
	- Injected HttpClient (more duplicated code)
	- Named clients
	- Typed clients

###### HttpClient Creation

![[Pasted image 20221229152605.png]]

###### Using HttpClient

![[Pasted image 20221229152642.png]]


###### Using Factory 

![[Pasted image 20221229152803.png]]

```C#
//In Program.cs (better approach donot need to set url everytime)
builder.Services.AddHttpClient("backend", client => 
	{
		client.BaseAddress = new Uri("https://localhost:7213");
	}							  
);
//We can have multiple clients
builder.Services.AddHttpClient("backend2", client => 
	{
		client.BaseAddress = new Uri("https://localhost:7213");
	}							  
);

//Typed HttpClient
builder.Services.AddHttpClient<IApiCaller, ApiCaller>(client => 
	{
		client.BaseAddress = new Uri("https://localhost:7213");
	}							  
);
```

