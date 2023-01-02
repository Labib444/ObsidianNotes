
```C#
// Use "Authorize" keyword to block unauthorized api calls
[HttpGet(Name = "GetWeatherForecast"), Authorize]
public IActionResult GetWeatherForecast(){
	...
}
```

```C#
//Add in top of the controller 
[ApiController]
[Route("[controller]")]
[Authorize]

//Allow Anonymous access
[HttpGet(Name = "GetWeatherForecast"), AllowAnonymous]
...
```

```C#
//For role based
[HttpGet(Name = "GetWeatherForecast", Authorize(Roles = "Admin"))]
```

###### In Program.cs

```C#
builder.Services.AddAuthetication(JwtBearerDefaults.AuthenticationScheme)
	.AddJwtBearer( options => {  
		options.TokenValidationParameters = new TokenValidationParameters { 
			ValidateIssuerSigningKey = true,
			IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(
				builder.Configuration.GetSection("AppSettings:Token").Value
			)),
			ValidateIssuer = false,
			ValidateAudience = false
		}
	} )

app.UseAuthentication();
app.UseAuthorization();
```

### If you want to use Authentication in Swagger

```C#
builder.Services.AddSwaggerGen(options => {

	options.AddSecurityDefinition("oauth2", new OpenApiSecurityScheme{
		Description = "Standard Authorization header using the Bearer scheme ("\bearer {token}\" )",
		In = ParameterLocation.Header,
		Name = "Authorization",
		Type = SecuritySchemeType.ApiKey
	})
	options.OperationFilter<SecurityRequirementsOperationFilter>();
})
```