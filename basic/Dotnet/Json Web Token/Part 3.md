
### Reading User Claims  (2 Methods)

### Method 1

```C#
public static User user = new User();

[HttpGet, Authorize]
public ActionResult<string> GetMe()
{
	var userName = User?.Identity?.Name;
	var userName2 = User.FindFirstValue(ClaimTypes.Name); 
	var role = User.FindFirstValue(ClaimTypes.Role);
	return Ok( new { userName, userName2, role } );
}
```

### Method 2 (using services) (best approach)

New Folder -> Name Services -> New Folder -> UserService -> Add Interface -> IUserService -> Add class -> UserService

- In Program.cs
builder.Services.AddScoped<IUserService, UserService>();

global using JwtWebApiTutorial.Services.UserService; (make it global so that we donot need to important again.)

builder.Services.AddHttpContextAccessor();

- In UserService Folder
```C#
public class UserService : IUserService
{

	public UserService(IHttpContextAccessor httpContextAccessor)
	{
		_httpContextAccessor = httpContextAccessor;
	}
	
	public string GetMyName()
	{
		var result = string.Empty;
		if( _httpContextAccessor.HttpContext != null )
		{
			result = _httpContextAccessor.HttpContext.User.FindFirstValue(ClaimTypes.Name);
			return result;
		}
	}
}
```

Now inject the service in the controller from the constructor


### Add Cors (if you have UI Framework like Angular)

```C#
builder.Services.AddCors( options => options.AddPolicy(
	name: "NgOrigins",
	policy =>
	{
		policy.WithOrigins("http://localhost:4200").AllowAnyMethod().AllowAnyHeader();
	}													  
) );

var app = builder.Build();
app.UseCors("NgOrigins");
```