https://www.tutorialsteacher.com/linq/linq-api

# LINQ

Before C# 2.0, we had to use a 'foreach' or a 'for' loop to traverse the collection to find a particular object.

```csharp
class Student
{
    public int StudentID { get; set; }
    public String StudentName { get; set; }
    public int Age { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        Student[] studentArray = { 
            new Student() { StudentID = 1, StudentName = "John", Age = 18 },
            new Student() { StudentID = 2, StudentName = "Steve",  Age = 21 },
            new Student() { StudentID = 3, StudentName = "Bill",  Age = 25 },
            new Student() { StudentID = 4, StudentName = "Ram" , Age = 20 },
            new Student() { StudentID = 5, StudentName = "Ron" , Age = 31 },
            new Student() { StudentID = 6, StudentName = "Chris",  Age = 17 },
            new Student() { StudentID = 7, StudentName = "Rob",Age = 19  },
        };

        Student[] students = new Student[10];

        int i = 0;

        foreach (Student std in studentArray)
        {
            if (std.Age > 12 && std.Age < 20)
            {
                students[i] = std;
                i++;
            }
        }
    }
}
```

Use of for loop is cumbersome, not maintainable and readable. C# 2.0 introduced delegate, which can be used to handle this kind of a scenario:

```csharp
delegate bool FindStudent(Student std);

class StudentExtension
{ 
    public static Student[] where(Student[] stdArray, FindStudent del)
    {
        int i=0;
        Student[] result = new Student[10];
        foreach (Student std in stdArray)
            if (del(std))
            {
                result[i] = std;
                i++;
            }

        return result;
    }
}
    
class Program
{
    static void Main(string[] args)
    {
        Student[] studentArray = { 
            new Student() { StudentID = 1, StudentName = "John", Age = 18 } ,
            new Student() { StudentID = 2, StudentName = "Steve",  Age = 21 } ,
            new Student() { StudentID = 3, StudentName = "Bill",  Age = 25 } ,
            new Student() { StudentID = 4, StudentName = "Ram" , Age = 20 } ,
            new Student() { StudentID = 5, StudentName = "Ron" , Age = 31 } ,
            new Student() { StudentID = 6, StudentName = "Chris",  Age = 17 } ,
            new Student() { StudentID = 7, StudentName = "Rob",Age = 19  } ,
        };

        Student[] students = StudentExtension.where(studentArray, delegate(Student std){
                return std.Age > 12 && std.Age < 20;
            });
        }
    }
}
```

So, with C# 2.0, you got the advantage of delegate in finding students with any criteria. You don't have to use a for loop to find students using different criteria. For example, you can use the same delegate function to find a student whose StudentId is 5 or whose name is Bill, as below:

```csharp
Student[] students = StudentExtension.where(studentArray, delegate(Student std) {
        return std.StudentID == 5;
    });

//Also, use another criteria using same delegate
Student[] students = StudentExtension.where(studentArray, delegate(Student std) {
        return std.StudentName == "Bill";
    });
```

The C# team felt that they still needed to make the code even more compact and readable. So they introduced the extension method, lambda expression, expression tree, anonymous type and query expression in C# 3.0. You can use these features of C# 3.0, which are building blocks of LINQ to query to the different types of collection and get the resulted element(s) in a single statement.

The example below shows how you can use LINQ query with lambda expression to find a particular student(s) from the student collection.

We can write LINQ queries for the classes that implement IEnumerable<T> or IQueryable<T> interface. LINQ queries uses extension methods for classes that implement IEnumerable or IQueryable interface. The Enumerable and Queryable are two static classes that contain extension methods to write LINQ queries.

## LINQ Query Syntax

There are two basic ways to write a LINQ query to IEnumerable collection or IQueryable data sources.

1) Query Syntax or Query Expression Syntax
2) Method Syntax or Method Extension Syntax or Fluent

### Query Syntax
Query syntax is similar to SQL (Structured Query Language) for the database. It is defined within the C# or VB code. The LINQ query syntax starts with from keyword and ends with select keyword. LINQ query syntax always ends with a Select or Group clause.

```csharp

// from <range variable> in <IEnumerable<T> or IQueryable<T> Collection>
// <Standard Query Operators> <lambda expression>
// <select or groupBy operator> <result formation>

// LINQ Query Syntax
var result = from s in stringList
            where s.Contains("Tutorials") 
            select s;
```

### Method Syntax
Method syntax (also known as fluent syntax) uses extension methods included in the Enumerable or Queryable static class, similar to how you would call the extension method of any class.

The compiler converts query syntax into method syntax at compile time.

```csharp
// LINQ Method Syntax
var result = stringList.Where(s => s.Contains("Tutorials"));
```

If you check the signature of the Where extension method, you will find the Where method accepts a predicate delegate as `Func<Student, bool>`. This means you can pass any delegate function that accepts a Student object as an input parameter and returns a Boolean value as shown in the below figure.

# Lambda Expression

C# 3.0(.NET 3.5) introduced the lambda expression along with LINQ. The lambda expression is a shorter way of representing anonymous method using some special syntax.

```csharp
// Anonymous Method:
delegate(Student s) { return s.Age > 12 && s.Age < 20; };

// The above anonymous method can be represented using a Lambda Expression in C# as:
s => s.Age > 12 && s.Age < 20

// You can wrap the parameters in parenthesis if you need to pass more than one parameter
(s, youngAge) => s.Age >= youngage;

// You can also give type of each parameters if parameters are confusing:
(Student s,int youngAge) => s.Age >= youngage;

// Lambda Expression without Parameter
() => Console.WriteLine("Parameter less lambda expression")

// Multiple Statements in Lambda Expression Body
(s, youngAge) =>
{
  Console.WriteLine("Lambda expression with multiple statements in the body");    
  Return s.Age >= youngAge;
}

// Declare Local Variable in Lambda Expression Body
s =>
{
   int youngAge = 18;
    Console.WriteLine("Lambda expression with multiple statements in the body");
    return s.Age >= youngAge;
}

// Assign Lambda Expression to Delegate
// The lambda expression can be assigned to Func<in T, out TResult> type delegate. The last parameter type in a Func delegate is the return type and rest are input parameters.

Func<Student, bool> isStudentTeenAger = s => s.age > 12 && s.age < 20;
Student std = new Student() { age = 21 };
bool isTeen = isStudentTeenAger(std);// returns false

// Unlike the Func delegate, an Action delegate can only have input parameters.

Action<Student> PrintStudentDetail = s => Console.WriteLine("Name: {0}, Age: {1} ", s.StudentName, s.Age);
Student std = new Student(){ StudentName = "Bill", Age=21};
PrintStudentDetail(std);//output: Name: Bill, Age: 21
```

## Standard Query Operators

Classification	| Standard Query Operators
Filtering	    |	Where, OfType
Sorting	        |	OrderBy, OrderByDescending, ThenBy, ThenByDescending, Reverse
Grouping	    |	GroupBy, ToLookup
Join	        |	GroupJoin, Join
Projection	    |	Select, SelectMany
Aggregation	    |	Aggregate, Average, Count, LongCount, Max, Min, Sum
Quantifiers	    |	All, Any, Contains
Elements 	    |	ElementAt, ElementAtOrDefault, First, FirstOrDefault, Last, LastOrDefault, Single, SingleOrDefault
Set 	        |	Distinct, Except, Intersect, Union
Partitioning	|	Skip, SkipWhile, Take, TakeWhile
Concatenation	|	Concat
Equality	    |	SequenceEqual
Generation	    |	DefaultEmpty, Empty, Range, Repeat
Conversion	    |	AsEnumerable, AsQueryable, Cast, ToArray, ToDictionary, ToList

```csharp
// **
// WHERE:Returns values from the collection based on a predicate function.
// **

var filteredResult = studentList.Where(s => s.Age > 12 && s.Age < 20);

// You can call the Where() extension method more than one time in a single LINQ query. Multiple where clauses. Having multiple where clauses definitely will have a performance impact if you have a large collection, and if you use all of the results from this query.

// Chaining the 2 clauses together causes the query to form a single delegate that gets run as the collection is enumerated. This results in one enumeration through the collection and one call to the delegate each time a result is returned. If you split them, things change. As your first where clause enumerates through the original collection, the second where clause enumerates its results.

// If you do decide to use multiple where clauses (splitted), placing the more restrictive clauses first will help quite a bit, since the remaining where clause will only run on the elements that pass the previous ones.

// **
// OFTYPE: Returns values from the collection based on a specified type. However, it will depend on their ability to cast to a specified type.
// **
var stringResult = mixedList.OfType<string>();



```






```csharp```
```csharp```
```csharp```
```csharp```
```csharp```
```csharp```
```csharp```
```csharp```
```csharp```
```csharp```
```csharp```
```csharp```






































