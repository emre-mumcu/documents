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

# Join Sample

```cs
var sonuc = _DbContext.Entity1.Join(
    _DbContext.Entity2,
    k => k.Id,
    i => i.Id,
    (k, i) => new {   })
;

```

The LINQ join operator allows us to join multiple tables on one or more columns (multiple columns). By default, they perform the inner join of the tables. 

## Query Syntax

The joins Queries are easier with the Query Syntax.

The following query joins Track and MediaType table using the Join query operator. The Join operator uses the Equals Keyword to compare the two or more columns. In this example, we use the column MediaTypeId. The query looks very similar to the normal database SQL Join Queries.

```csharp
    //Query Syntax
    using (ChinookContext db = new ChinookContext())
    {
        var Track = (from o in db.Track
                        join i in db.MediaType
                        on o.MediaTypeId equals i.MediaTypeId
                        select new
                        {
                            Name = o.Name,
                            Composer = o.Composer,
                            MediaType = i.Name
                        }).Take(5);
 
        foreach (var t in Track)
        {
            Console.WriteLine("{0} {1} {2}", t.Name, t.Composer, t.MediaType);
        }
    }

```

## Method Syntax

The method query syntax uses the join method. join method is an extension method, which takes the following syntax.

```csharp
public static IEnumerable<TResult> Join<TOuter, TInner, TKey, TResult>(
    this IEnumerable<TOuter> outer,
    IEnumerable<TInner> inner,
    Func<TOuter, TKey> outerKeySelector,
    Func<TInner, TKey> innerKeySelector,
    Func<TOuter, TInner, TResult> resultSelector
)
```
### Step by Step Composition

```csharp

// IEnumerable<TOuter> outer:
// The first sequence to join. In our case it is Track table.
var Track = db.Track

// IEnumerable<TInner> inner
// The sequence to join to the first sequence. Here we use MediaType table, which we are going to join to the Track table.
.Join(db.MediaType,

// Func<TOuter, TKey> outerKeySelector
// A function to extract the join key from the first sequence (Track table). Here we use a lambda expression l => l.MediaTypeId. We use the MediaTypeId of the Track table is to join the two tables
o => o.MediaTypeId,

// Func<TInner, TKey> innerKeySelector
// A function to extract the join key from the second sequence (MediaType table). This is similar to the outerKeySelector, but the specify the key to be used from the second table. In our case is MediaTypeId field from MediaTypetable.
i => i.MediaTypeId,

// Func<TOuter, TInner, TResult>
// A function to create a result element from two matching elements. Here the two matching elements are TOuter which is our Track table ( o) & TInner which is MediaType table (i).
// We use the projection to an anonymous type to return the result.
(o, i) => new {
    Name = o.Name,
    Composer = o.Composer,
    MediaType = i.Name
}

// The final query is as follows
//Method Syntax
using (ChinookContext db = new ChinookContext())
{

    var Track = db.Track
        .Join(db.MediaType,
            o => o.MediaTypeId,
            i => i.MediaTypeId,
            (o, i) =>
            new
            {
                Name = o.Name,
                Composer = o.Composer,
                MediaType = i.Name
            }
        ).Take(5);

    foreach (var t in Track)
    {
        Console.WriteLine("{0} {1} {2}", t.Name, t.Composer, t.MediaType);
    }

}


// Here is an another example, where we use the join clause to join customer & employee table. Both in method & query syntax.
 //Method Syntax
using (ChinookContext db = new ChinookContext())
{
    var Track = db.Customer
        .Join(db.Employee,
            f => f.SupportRepId,
            s => s.EmployeeId,
            (f, s) =>
            new
            {
                CustomerName = f.FirstName,
                CustomerState = f.State,
                EmployeeName = s.FirstName,
                EmployeeState = s.State,
            }
        ).Take(5);

    foreach (var t in Track)
    {
        Console.WriteLine("{0} {1} {2} {3}", t.CustomerName, t.CustomerState, t.EmployeeName, t.EmployeeState);
    }
}

```

## LINQ Join on Multiple Columns

To join two tables on more than one column (join by using composite keys), we need to define an anonymous type with the values we want to compare

```csharp
var Track = db.Customer
    .Join(db.Employee,
        f => new { f1 = f.SupportRepId.Value, f2 = f.State },
        s => new { f1 = s.EmployeeId, f2 = s.State },
        (f, s) =>
        new
        {
            CustomerName = f.FirstName,
            CustomerState = f.State,
            EmployeeName = s.FirstName,
            EmployeeState = s.State,
        }
    ).Take(5);
```

## Joining three or more Tables

The following queries demonstrate the use of Join queries between three or more tables. The query below queries for all invoices of track Bohemian Rhapsody with their qty & amount. This query involves joining three tables Track, InvoiceLine & Invoice.

You can keep repeat the join again for more tables. Finally use the ToList method execute the query.

```csharp

//Method Syntax
    using (ChinookContext db = new ChinookContext())
    {
        var Track = db.Track
           .Join(db.InvoiceLine,
                f => f.TrackId, s => s.TrackId,
                (f, s) => new { TrackName = f.Name, TrackId = f.TrackId, InvoiceId = s.InvoiceId, Quantity = s.Quantity, UnitPrice = s.UnitPrice }
                 )
                .Join(db.Invoice,
                     f => f.InvoiceId, s => s.InvoiceId,
                     (f, s) => new { TrackName = f.TrackName, TrackId = f.TrackId, InvoiceId = f.InvoiceId, InvoiceDate = s.InvoiceDate, Quantity = f.Quantity, UnitPrice = f.UnitPrice }
                     ).Where(r => r.TrackName == "Bohemian Rhapsody")
                    .ToList();
 
        foreach (var r in Track)
        {
            Console.WriteLine("{0} {1} {2} {3}", r.TrackName, r.InvoiceDate, r.Quantity, r.UnitPrice);
        }
    }
```

## Left Join

The EF Core converts to above joins into an INNER JOIN. But the other most used join is a SQL left join. To use a left join we use the methodDefaultIfEmpty in the Query Syntax.

```csharp
var Track = db.Customer.AsNoTracking(!track)
    .Join(db.Employee.DefaultIfEmpty(),
        f => new { f1 = f.SupportRepId.Value, f2 = f.State },
        s => new { f1 = s.EmployeeId, f2 = s.State },
        (f, s) =>
        new
        {
            CustomerName = f.FirstName,
            CustomerState = f.State,
            EmployeeName = s.FirstName,
            EmployeeState = s.State,
        }
    ).Take(5);

    // .ConfigureAwait(false); ??
```