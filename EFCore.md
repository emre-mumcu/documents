# Entity Framework Core

Run the following command to install EF Core .NET CLI tools:

```zsh
dotnet tool install --global dotnet-ef
dotnet tool update --global dotnet-ef
```

To install EF Core, you install the package for the EF Core database provider(s) you want to target.

```zsh
% dotnet add package Microsoft.EntityFrameworkCore.SqlServer
% dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

Before you can use the EF Core tools on a specific project, you'll need to add the Microsoft.EntityFrameworkCore.Design package to it. Microsoft.EntityFrameworkCore.Design is a DevelopmentDependency package. Microsoft.EntityFrameworkCore.Design contains all the design-time logic for Entity Framework Core.

```zsh
% dotnet add package Microsoft.EntityFrameworkCore.Design
```

# Code First

```zsh
% dotnet ef migrations add Mig0  [-c ProjectHavingDbContext] [-s StartupProject] [-o Migrations/Folder]
% dotnet ef migrations add Mig0 -o App_Data\Migrations
% dotnet ef database update  [-c ProjectHavingDbContext] [-s StartupProject] 
% dotnet ef database drop
% dotnet ef migrations remove

% dotnet ef migrations script -o script.sql
% dotnet ef migrations script Mig0 Mig1 -o script.sql [-c ProjectHavingDbContext] [-s StartupProject] 
% dotnet ef migrations script --help
```
