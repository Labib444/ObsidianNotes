
### Part 1

- Caching
- Asynchronous programming
- HttpContext and HttpClient, and Form handling
- Diagnostics, Benchmarking, and Load Testing
- Optimizing HTTP content
- Optimizing memory usage

### Part 2

visit to learn who is the fasted framework.
www.techempower.com

### Part 3
- Understand "hot code paths"
- Focus which places where you page requires the most data.
- batch it and asynchronous the "hot code paths"

### Part 4

- Use System.Text.Json
![[Pasted image 20221227003447.png]]

!Important!
Optimize code where heavy usage is done donot try to optimize everything else the code will become extremely complex and unreadable.

```C#

var response = await _apiClient.GetAsync($"Product?category={cat}");

if( response.IsSuccessStatusCode )
{
	var jsonContent = await response.Content.ReadAsStringAsync();

	//Method: Newtonsoft.Json (Normal Method)
	Products = JsonConvert.DeserializeObject<List<ProductModel>>(jsonContent); 

	//Method: System.Text.Json (Better Method)
	Products = JsonSerializer.Deserialize<List<ProductModel>>(jsonContent, 
		new  JsonSerializerOptions(defaults:JsonSerializerDefaults.Web));

	//Method: (Best Method)
	Products = await response.Content.ReadFromJsonAsync<List<ProductModel>>();
}

```

```C#
//Method: Fastest Best Method
//In ProductModeel Class 
//Add this line
[JsonSourceGenerationOptions(PropertyNamingPolicy = JsonKnownNamingPolicy.CamelCase)]
[JsonSerializable(type(List<ProductModel>))]
public partial class SourceGenerationContext : JsonSerializerContext
```

```C#
var response = await _apiClient.GetAsync($"Product?category={cat}");

if( response.IsSuccessStatusCode )
{
	Products = JsonSerializer.Deserialize(jsonContent, SourceGenerationContext.Default.ListProductModel);
}
```

### Part 5

###### Donot Do

```C#
public class ProductModel
{
	...
	...
	...
	public virtual ProductRating productRating {get; set;}
}
```

```C#
protected override void OnConfiguring(DbContextOptionsBuilder options)
{
	options
		.UseSqlite($"Data Source={DbPath}")
		.UseLazyLoadingProxies() //EntityFrameworkCore.Proxies
		.LogTo(Console.WriteLine, LogLevel.Information);
}
```


###### Do

- Remove virtual keyword
- Remove UseLazyLoadingProxies()
- Use Include() of Entity FrameWork, it acts like join.

```C#
//Disable tracking of EF Core.
//If your only querying to read data and updating from other methods (such as other dbContext) then turn off tracking.

protected override void OnConfiguring(DbContextOptionsBuilder options)
{
	options
		.UseSqlite($"Data Source={DbPath}")
		.UseQueryTrackingBehaviour(QueryTrackingBehaviour.NoTracking)
		.LogTo(Console.WriteLine, LogLevel.Information);
}
```