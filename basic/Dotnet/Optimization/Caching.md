
### Part 1

Cache common heavy results. (Remote read operations, used frequently, high overhead)
Response content (Assets or content sent to browser, especially frequently-sent static content)

- What to Cache (donot need to cache all)
- When to Cache (When you need to)

### Part 2

![[Pasted image 20221227011608.png]]

### Part 3

In-memory caching with expiration
Distributed caching with expiration
	- Serialization matters
	- Explicit cache invalidation

### Part 4 (In-Memory Caching)

- Simeple Easy and Fast
- Too much caching may reduce your server memory.
-  Donot store heavy data in such caches.

```C#
//Instantiate IMemoryCache in the constructor
try{
	var cacheKey = $"products_{category}";
	if(!_memoryCache.TryGetValue(cacheKey, out List<Product> results))
	{
		Thread.Sleep(5000);
		results = await _ctx.Products
					.Where(p => p.Category == category || category == "aLL")
					.Include(p => p.Rating).ToListAsync();
		_memoryCache.Set(cacheKey, results, TimeSpan.FromMinutes(2));
	}
}
```

```C#
//In Program.cs
builder.Services.AddMemoryCache();
```

### Part 5

IDistributedCache