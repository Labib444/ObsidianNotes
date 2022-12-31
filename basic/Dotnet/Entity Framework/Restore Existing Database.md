### In Visual Studio

*Need to install EntityFrameworkCore.Designs*

```
Scaffold-DbContext [-Connection] [-Provider] [-OutputDir] [-Context] [-Schemas>] 
[-Tables>] 
[-DataAnnotations] [-Force] [-Project] [-StartupProject] [<CommonParameters>]
```

```
Scaffold-DbContext

'Server=.\SQLEXPRESS;
Initial Catalog=School;
Trusted_Connection=True;
TrustServerCertificate=True;' (Connection)

Microsoft.EntityFrameworkCore.SqlServer (Provider)
-OutputDir Models 
-ContextDir Data
```

### How it works

- Reverse engineering starts by reading the database schema. It reads information about tables, columns, constraints, and indexes.

- Next, it uses the schema information to create an EF Core model. Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.

- Finally, the model is used to generate code. The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.

<h3 style="color: red;">Limitations</h3> 
- Not everything about a model can be represented using a database schema. For example, information about inheritance hierarchies, owned types, and table splitting are not present in the database schema. Because of this, these constructs will never be reverse engineered.

- In addition, some column types may not be supported by the EF Core provider. These columns won't be included in the model.

- You can define concurrency tokens in an EF Core model to prevent two users from updating the same entity at the same time. Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.

- Prior to EF Core 6, the C# 8 nullable reference type feature wasn't supported in reverse engineering: EF Core always generated C# code that assumed the feature is disabled. For example, nullable text columns were scaffolded as a property with type string , not string?, with either the Fluent API or Data Annotations used to configure whether a property is required or not. If using an older version of EF Core, you can still edit the scaffolded code and replace these with C# nullability annotations.

### In VS Code

https://learn.microsoft.com/en-us/ef/core/managing-schemas/scaffolding/?tabs=vs