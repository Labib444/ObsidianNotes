
### Entity Framework Core

EF Core can serve as an object-relational mapper (O/RM), which:
-   Enables .NET developers to work with a database using .NET objects.
-   Eliminates the need for most of the data-access code that typically needs to be written.

With EF Core, data access is performed using a model. A model is made up of entity classes and a context object that represents a session with the database. The context object allows querying and saving data.

EF supports the following model development approaches:
-   Generate a model from an existing database.
-   Hand code a model to match the database.
-   Once a model is created, use EF Migrations to create a database from the model. Migrations allow evolving the database as the model changes.

```C#
using System.Collections.Generic;
using Microsoft.EntityFrameworkCore;

namespace Intro;

public class BloggingContext : DbContext
{
	public DbSet<Blog> Blogs { get; set; }
	public DbSet<Post> Posts { get; set; }
	protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder) {
		 optionsBuilder.UseSqlServer( @"Server= 
			 (localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True"); 
	}
}
```

- Instances of your entity classes are retrieved from the database using LINQ.
- Data is created, deleted, and modified in the database using instances of your entity classes.