- EF Core is a [.NET Standard 2.0](https://learn.microsoft.com/en-us/dotnet/standard/net-standard) library. So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run. EF Core can also be referenced by other .NET Standard 2.0 libraries.

- For example, you can use EF Core to develop apps that target .NET Core. Building .NET Core apps requires the .NET SDK.

- Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems. Make sure an [EF Core database provider](https://learn.microsoft.com/en-us/ef/core/providers/) is available that supports the right environment for your application.

### Installing from .NET CLI

```.NET CLI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer

//for specific version
dotnet add package Microsoft.EntityFrameworkCore.SqlServer -v 2.2.0
```

### Installing from Visual Studio

1. Project > Manage NuGet Packages > (click) Browse > (install)

### Installing from Nuget Package Manager Console

```Powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer

#For specific version
Install-Package Microsoft.EntityFrameworkCore.SqlServer -Version 2.2.0
```

# Getting EF Core Tools

### Two types

-  .NET Core command-line interface (CLI) tools
-  Package Manager Console (PMC) tools

### .NET Core command-line interface (CLI) tools

These commands start with "dotnet ef"

###### Install a local tool

If you want to install a tool for local access only (for the current directory and subdirectories), you must add the tool to a tool manifest file. To create a tool manifest file, run the `dotnet new tool-manifest` command:

```.NET CLI
dotnet new tool-manifest (creating manifest file for local use)
dotnet tool install dotnet-ef (don't use --global --tool-path)
dotnet tool restore (install tools from manifest file when creating new project.)
dotnet tool update (updating tools)
```

###### Install a global tool

```.NET CLI
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.Design
```

### Package Manager Console (PMC) tools

These commands start with "Add-Migration, Update-Database". Using PMC is better in Visual Studio. Reason given below:

-   They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.
-   They automatically open files generated by the commands in Visual Studio after the command is completed.

```Powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```