# .NET Core - New database

### Prerequisites
The following prerequisites are needed to complete this walkthrough:
* An operating system that supports .NET Core.
* The .NET Core SDK 1.1 and later.

### Create a new project
1. `cd [folder]`
1. `dotnet new console`

### Install Entity Framework Core

We are going to use SQLite for this example.

* Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design

```
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Manually edit `dotnetcore-ef7.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet

Note: A future version of `dotnet` will support DotNetCliToolReferences via `dotnet add tool`

`dotnetcore-ef7.csproj` should now contain the following:

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="1.1.1" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="1.1.1" PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="1.0.1" />
  </ItemGroup>
</Project>
```

Note: The version numbers used above were correct at the time of publishing.
* Run `dotnet restore` to install the new packages.

GO and create the models!

## Create the database

Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.

* Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.
* Run `dotnet ef database update` to apply the new migration to the database. This command creates the database before applying migrations.

Notes:
* When using relative paths with SQLite, the path will be relative to the application's main assembly. In this sample, the main binary is `bin/Debug/netcoreapp1.1/dotnetcore-ef7.dll`, so the SQLite database will be in `bin/Debug/netcoreapp1.1/blogging.db`.

GO and use the model!!

## To test the App run: `dotnet run`

## Changing the model

* If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new migration to make the corresponding schema changes to the database. Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.
* EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.
* SQLite does not support all migrations (schema changes) due to limitations in SQLite. See SQLite Limitations. For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.