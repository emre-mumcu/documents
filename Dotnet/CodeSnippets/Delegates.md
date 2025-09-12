# Delegates

A delegate is a type that safely encapsulates a method, similar to a function pointer in C and C++. Unlike C function pointers, delegates are object-oriented, type safe, and secure.

The following example declares a delegate named Callback that can encapsulate a method that takes a string as an argument and returns void:

```csharp
public delegate void Callback(string message);

// Create a method for a delegate.
public static void DelegateMethod(string message)
{
    Console.WriteLine(message);
}

// Instantiate the delegate.
Callback handler = DelegateMethod;

// Call the delegate.
handler("Hello World");
```

Invoking a delegate calls the method attached to the delegate instance. The parameters passed to the delegate by the caller are passed to the method. The delegate returns the return value, if any, from the method.

Delegate types are derived from the Delegate class in .NET. Delegate types are sealed, they can't be derived from, and it isn't possible to derive custom classes from Delegate.

Because the instantiated delegate is an object, it can be passed as an argument, or assigned to a property. A method can accept a delegate as a parameter, and call the delegate at some later time. This is known as an asynchronous callback, and is a common method of notifying a caller when a long process completes.

A delegate can call more than one method when invoked, referred to as multicasting. To add an extra method to the delegate's list of methods—the invocation list—simply requires adding two delegates using the addition or addition assignment operators ('+' or '+='). For example:

```csharp
var obj = new MethodClass();

Callback d1 = obj.Method1;
Callback d2 = obj.Method2;
Callback d3 = DelegateMethod;

//Both types of assignment are valid.
Callback allMethodsDelegate = d1 + d2;
allMethodsDelegate += d3;
```

When allMethodsDelegate is invoked, all three methods are called in order. If the delegate uses reference parameters, the reference is passed sequentially to each of the three methods in turn, and any changes by one method are visible to the next method. When any of the methods throws an exception that isn't caught within the method, that exception is passed to the caller of the delegate. No subsequent methods in the invocation list are called. If the delegate has a return value and/or out parameters, it returns the return value and parameters of the last method invoked. To remove a method from the invocation list, use the subtraction or subtraction assignment operators (- or -=). For example:

```csharp
//remove Method1
allMethodsDelegate -= d1;

// copy AllMethodsDelegate while removing d2
Callback oneMethodDelegate = (allMethodsDelegate - d2)!;
```

Because delegate types are derived from System.Delegate, the methods and properties defined by that class can be called on the delegate. For example, to find the number of methods in a delegate's invocation list, you can write:

```csharp
int invocationCount = d1.GetInvocationList().GetLength(0);
```

Multicast delegates are used extensively in event handling. 

## Variance in Delegates

.NET Framework 3.5 introduced variance support for matching method signatures with delegate types in all delegates in C#. This means that you can assign to delegates not only methods that have matching signatures, but also methods that return more derived types (covariance) or that accept parameters that have less derived types (contravariance) than that specified by the delegate type. This includes both generic and non-generic delegates.

```csharp
public class First { }  
public class Second : First { }  
public delegate First SampleDelegate(Second a);  
public delegate R SampleGenericDelegate<A, R>(A a);  

// Matching signature.  
public static First ASecondRFirst(Second second)  
{ return new First(); }  
  
// The return type is more derived.  
public static Second ASecondRSecond(Second second)  
{ return new Second(); }  
  
// The argument type is less derived.  
public static First AFirstRFirst(First first)  
{ return new First(); }  
  
// The return type is more derived
// and the argument type is less derived.  
public static Second AFirstRSecond(First first)  
{ return new Second(); }  


// Assigning a method with a matching signature
// to a non-generic delegate. No conversion is necessary.  
SampleDelegate dNonGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type
// and less derived argument type to a non-generic delegate.  
// The implicit conversion is used.  
SampleDelegate dNonGenericConversion = AFirstRSecond;  
  
// Assigning a method with a matching signature to a generic delegate.  
// No conversion is necessary.  
SampleGenericDelegate<Second, First> dGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type
// and less derived argument type to a generic delegate.  
// The implicit conversion is used.  
SampleGenericDelegate<Second, First> dGenericConversion = AFirstRSecond;
```

## Variance in Generic Type Parameters

In .NET Framework 4 or later you can enable implicit conversion between delegates, so that generic delegates that have different types specified by generic type parameters can be assigned to each other, if the types are inherited from each other as required by variance.

To enable implicit conversion, you must explicitly declare generic parameters in a delegate as covariant or contravariant by using the in or out keyword.

```csharp
// Type T is declared covariant by using the out keyword.  
public delegate T SampleGenericDelegate <out T>();  
  
public static void Test()  
{  
    SampleGenericDelegate <String> dString = () => " ";  
  
    // You can assign delegates to each other,  
    // because the type T is declared covariant.  
    SampleGenericDelegate <Object> dObject = dString;
}
```

If you use only variance support to match method signatures with delegate types and do not use the in and out keywords, you may find that sometimes you can instantiate delegates with identical lambda expressions or methods, but you cannot assign one delegate to another.

```csharp
public delegate T SampleGenericDelegate<T>();  
  
public static void Test()  
{  
    SampleGenericDelegate<String> dString = () => " ";  
  
    // You can assign the dObject delegate  
    // to the same lambda expression as dString delegate  
    // because of the variance support for
    // matching method signatures with delegate types.  
    SampleGenericDelegate<Object> dObject = () => " ";  
  
    // The following statement generates a compiler error  
    // because the generic type T is not marked as covariant.  
    // SampleGenericDelegate <Object> dObject = dString;  
  
}
```

## Generic Delegates That Have Variant Type Parameters in .NET

.NET Framework 4 introduced variance support for generic type parameters in several existing generic delegates:

* Action delegates from the System namespace, for example, Action<T> and Action<T1,T2>
* Func delegates from the System namespace, for example, Func<TResult> and Func<T,TResult>
* The Predicate<T> delegate
* The Comparison<T> delegate
* The Converter<TInput,TOutput> delegate


# Action<T> Delegate

Encapsulates a method that has a single parameter and does not return a value.

Type Parameters T: The type of the parameter of the method that this delegate encapsulates. This type parameter is contravariant. That is, you can use either the type you specified or any type that is less derived.

```csharp
List<string> names = new List<string>();
names.Add("Bruce");
names.Add("Alfred");
names.Add("Tim");
names.Add("Richard");

// Display the contents of the list using the Print method.
names.ForEach(Print);

// The following demonstrates the anonymous method feature of C#
// to display the contents of the list to the console.
names.ForEach(delegate(string name) {
    Console.WriteLine(name);
});

void Print(string s) {
    Console.WriteLine(s);
}
```

# Func<TResult> Delegate

Encapsulates a method that has no parameters and returns a value of the type specified by the TResult parameter.

Type Parameters TResult: The type of the return value of the method that this delegate encapsulates. This type parameter is covariant. That is, you can use either the type you specified or any type that is more derived.

# Predicate<T> Delegate

Represents the method that defines a set of criteria and determines whether the specified object meets those criteria.

Type Parameters T: The type of the object to compare. This type parameter is contravariant. That is, you can use either the type you specified or any type that is less derived.

Return Value Boolean: true if obj meets the criteria defined within the method represented by this delegate; otherwise, false.

# Comparison<T> Delegate

Represents the method that compares two objects of the same type.

ype Parameters T: The type of the objects to compare. This type parameter is contravariant. That is, you can use either the type you specified or any type that is less derived. 

# Converter<TInput,TOutput> Delegate

Represents a method that converts an object from one type to another type.

Type Parameters TInput: The type of object that is to be converted. This type parameter is contravariant. That is, you can use either the type you specified or any type that is less derived.







































# References

* https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/using-delegates
* https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates













AddMvc(this IServiceCollection services, Action<MvcOptions> setupAction) metodunda ikinci parametre bir delegate (Action<MvcOptions>). Yani MvcOptions tipinden bir nesneyi alıp üzerinde işlem yapabilen bir metot referansı.

1. Lambda Expression (inline)

builder.Services.AddMvc(options =>
{
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute());
    options.MaxModelBindingCollectionSize = 1000;
});

2. Named Method (ayrı metot tanımlayıp parametre geçme)

void ConfigureMvc(MvcOptions options)
{
    options.Filters.Add(new RequireHttpsAttribute());
    options.SuppressAsyncSuffixInActionNames = false;
}

builder.Services.AddMvc(ConfigureMvc);

Burada ConfigureMvc metodu bir Action<MvcOptions> gibi davranıyor.

3. Delegate Instance (explicit Action<MvcOptions> oluşturma)

Action<MvcOptions> setup = options =>
{
    options.RespectBrowserAcceptHeader = true; // content negotiation
};

builder.Services.AddMvc(setup);

4. Anonim Method (delegate keyword ile)

Eski tarz ama geçerli:

builder.Services.AddMvc(delegate(MvcOptions options)
{
    options.EnableEndpointRouting = false;
});

5. Birden Fazla Delegate Zincirleme

C#’ta Action multicast olduğundan, birden fazla konfigürasyonu ekleyip zincirleyebilirsin:

Action<MvcOptions> configureFilters = o => o.Filters.Add(new AuthorizeFilter());
Action<MvcOptions> configureBinding = o => o.MaxModelBindingCollectionSize = 2000;

builder.Services.AddMvc(configureFilters + configureBinding);












1. A method that takes an Action

Action is a delegate that represents a method that returns void.

void Execute(Action action)
{
    action(); // invoke the delegate
}


You can call it in several ways:

// Passing a lambda
Execute(() => Console.WriteLine("Hello!"));

// Passing a method group
void SayHi() => Console.WriteLine("Hi there!");
Execute(SayHi);


2. A method that takes an Action<T>

If the delegate has parameters, you can pass them via lambda or method group.

void ExecuteWithParam(Action<string> action)
{
    action("ChatGPT");
}


Usage:

// Lambda inline
ExecuteWithParam(name => Console.WriteLine($"Hello {name}"));

// Method group
void PrintName(string name) => Console.WriteLine($"Hello {name}");
ExecuteWithParam(PrintName);


3. A method that takes a Func<T>

Func<T> represents a method that returns a value.

void RunCalculation(Func<int, int, int> func)
{
    int result = func(5, 3);
    Console.WriteLine($"Result: {result}");
}


Usage

// Lambda
RunCalculation((a, b) => a + b);

// Method group
int Multiply(int x, int y) => x * y;
RunCalculation(Multiply);


4. Custom delegate

You can also define your own delegate type:

delegate double MathOp(double a, double b);

void Compute(MathOp op)
{
    double result = op(10, 5);
    Console.WriteLine($"Result: {result}");
}

Usage


// Lambda
Compute((x, y) => x / y);

// Method group
double Subtract(double a, double b) => a - b;
Compute(Subtract);



