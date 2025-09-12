# MVC Builder

In ASP.NET Core, we can configure the MVC (Model-View-Controller) and Razor Pages services by calling methods such as AddControllers(), AddMvc(), AddControllersWithViews(), and AddRazorPages() on the IServiceCollection. When building an ASP.NET Core application, choosing the right service configuration method is crucial for optimizing functionality and performance.

These methods are implemented as extension methods on the IServiceCollection interface, which is used to register services in the ASP.NET Core dependency injection container. They are available with two overloads:

* Without parameters: Registers default services.
* With a parameterized Options object: Allows customization of the registered services.

## Features of All Available Methods





![alt text](../../Images/MvcBuilder.PNG "Title")