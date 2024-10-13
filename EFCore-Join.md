# EFCore Joining Entities

https://www.tektutorialshub.com/entity-framework-core/join-query-in-ef-core/

The LINQ join operator allows us to join multiple entities (tables) on one or more properties (columns). By default, they perform the inner join of the tables.

It is always advisable to use navigational properties to query the related data. You should use the Join Query operators only if the tables do not have any navigational properties defined on them or you want to fine-tune the generated queries for performance benefits.

## Query Syntax

The following query joins Track and table using the Join query operator. The Join operator uses the Equals Keyword to compare the two or more columns. In this example, we use the column . The query looks very similar to the normal database SQL Join Queries.

```cs
//Query Syntax
using (ChinookContext db = new ChinookContext())
{
    var Track = (
        from o in db.Track
        join i in db.MediaType
        on o.MediaTypeId equals i.MediaTypeId select new
        {
            Name = o.Name, Composer = o.Composer, MediaType = i.Name
        }
    )
    .Take(5);
}
```

## Method Syntax

```cs
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
        )
    .Take(5);
}
```

## LINQ Join on Multiple Columns

```cs
// Query Syntax
var result = (from m1 in db.model1
              join m2 in db.model2
               on        new { m1.field1 , m1.field2 } 
                  equals new { m2.field1 , m2.field2 }
             select new
              {
                field1 = m1.field1,
                field2 = m1.field2,
                someField = m2.someField
             }).ToList();

var Track = (from f in db.Customer
                join s in db.Employee
                on new { f1 = f.SupportRepId.Value, f2 = f.State } equals new { f1 = s.EmployeeId, f2 = s.State }
                select new
                {
                    CustomerName = f.FirstName,
                    CustomerState = f.State,
                    EmployeeName = s.FirstName,
                    EmployeeState = s.State,
                }).Take(5);             

// Method Syntax

```

## Joining three or more Tables

```cs
// Query Syntax
using (ChinookContext db = new ChinookContext())
    {
 
        var Track = (from t in db.Track
                        join il in db.InvoiceLine
                        on t.TrackId equals il.TrackId
                        join i in db.Invoice
                        on il.InvoiceId equals i.InvoiceId
                        where t.Name == "Bohemian Rhapsody"
                        select (new
                        {
                            TrackName = t.Name,
                            TrackId = t.TrackId,
                            InvoiceId = i.InvoiceId,
                            InvoiceDate = i.InvoiceDate,
                            Quantity = il.Quantity,
                            UnitPrice = il.UnitPrice
                        })
                    ).ToList(); 
    }

// Method Syntax
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
    }

// The query can also be written as shown below. Notice the difference in projection of the first join and this one.

var Track = db.Track
        .Join(db.InvoiceLine,
                f => f.TrackId, s => s.TrackId,
                (Track, InvoiceLine) => new { Track, InvoiceLine }       //Projecting entire row from both tables
            )
            .Join(db.Invoice,
                f => f.InvoiceLine.InvoiceId, s => s.InvoiceId,
                (f, s) => new { TrackName = f.Track.Name, TrackId = f.Track.TrackId, InvoiceId = f.InvoiceLine.InvoiceId, InvoiceDate = s.InvoiceDate, Quantity = f.InvoiceLine.Quantity, UnitPrice = f.InvoiceLine.UnitPrice }
                )
        .Where(r => r.TrackName == "Bohemian Rhapsody")
        .ToList();

```

## Left Join

The EF Core converts to above joins into an INNER JOIN. But the other most used join is a SQL left join. To use a left join we use the method `DefaultIfEmpty` in the Query Syntax.


IMplement Code Here