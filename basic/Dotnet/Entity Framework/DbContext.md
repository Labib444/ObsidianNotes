### What is DbContext

A DbContext instance represents a session with the database and can be used to query and save instances of your entities. DbContext is a combination of the Unit Of Work and Repository patterns.

### Cannot run multiple quries concurrently

Entity Framework Core does not support multiple parallel operations being run on the same DbContext instance. This includes both parallel execution of async queries and any explicit concurrent use from multiple threads. Therefore, always await async calls immediately, or use separate DbContext instances for operations that execute in parallel.


### Public and Private DbSet: TEntity

Typically you create a class that derives from DbContext and contains DbSet properties for each entity in the model. If the DbSet properties have a public setter, they are automatically initialized when the instance of the derived context is created.

### Configuring options in DbContext: TContext

Override the OnConfiguring(DbContextOptionsBuilder) method to configure the database (and other options) to be used for the context. Alternatively, if you would rather perform configuration externally instead of inline in your context, you can use DbContextOptionsBuilder (or DbContextOptionsBuilder) to externally create an instance of DbContextOptions (or DbContextOptions) and pass it to a base constructor of DbContext.

### OnModelCreating(ModelBuilder)

The model is discovered by running a set of conventions over the entity classes found in the DbSet properties on the derived context. To further configure the model that is discovered by convention, you can override the OnModelCreating(ModelBuilder) method.

### Use of Events 

Following events has been introduced

-   ***DbContext.SavingChanges***
		This event is raised at the start of SaveChanges or SaveChangesAsync

-   ***DbContext.SavedChanges***
		This event is raised at the end of a successful SaveChanges or SaveChangesAsync

-   ***DbContext.SaveChangesFailed***
		This event is raised at the end of a failed SaveChanges or SaveChangesAsync

-   ***SavingChanges***
	-   Called at the start of DbContext.SaveChanges

-   ***SavingChangesAsync***
	-   Called at the start of DbContext.SaveChangesAsync.

-   ***SavedChanges***
	-   Called at the end of DbContext.SaveChanges.

-   ***SavedChangesAsync***
	-   Called at the end of DbContext.SaveChangesAsync.

-   ***SaveChangesFailed***
	-   Called when there is an exception thrown from DbContext.SaveChanges.

-   ***SaveChangesFailedAsync***
	-   Called when there is an exception thrown from DbContext.SaveChangesAsync.


### Use of Events Example

```C#
static async Task Main(string[] args)  
{  
    using var context = new MyDbContext();  
    await context.Database.EnsureDeletedAsync();  
    await context.Database.EnsureCreatedAsync();  
   
    context.SavingChanges += Context_SavingChanges;  
    context.SavedChanges += Context_SavedChanges;  
    context.SaveChangesFailed += Context_SaveChangesFailed;  
   
    var john = new Student { Name = "John" };  
    var jane = new Student { Name = "Jane" };  
   
    context.AddRange(john, jane);  
    await context.SaveChangesAsync();  
} 
```

```C#
private static void Context_SavingChanges(object sender, SavingChangesEventArgs args)  
{  
    Console.WriteLine($"Starting saving changes.");  
}  
```

```C#
private static void Context_SavedChanges(object sender, SavedChangesEventArgs args)  
{  
    Console.WriteLine($"Saved {args.EntitiesSavedCount} No of changes.");  
}  
```

```C#
private static void Context_SaveChangesFailed(object sender, SaveChangesFailedEventArgs args)  
{  
    Console.WriteLine($"Saving failed due to {args.Exception}.");  
}
```

