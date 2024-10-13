# AsEnumerable

AsEnumerable() is a method for strongly-typed collections (like int[], string[]) and is available in the System.Linq namespace. 

However, Array, for example, is non-generic, and you cannot directly use AsEnumerable() on it. If you're dealing with an Array (which is not strongly typed like int[] or string[]), you can use Cast<T>() from LINQ to convert it to an IEnumerable<T>.

```cs

```

# IConfiguration

```cs
IConfiguration configuration = new ConfigurationBuilder()
    .AddJsonFile("appsettings.json")
    .AddJsonFile($"appsettings.{environment}.json")
    .AddUserSecrets<Program>()
    .Build();

var myConfiguration = configuration.Get<MyConfiguration>();
```

# Kestrel Endpoint Configuration

New ASP.NET Core projects are configured to bind to a random HTTP port between 5000-5300 and a random HTTPS port between 7000-7300. The selected ports are stored in the generated Properties/launchSettings.json file and can be modified by the developer. The launchSetting.json file is only used in local development. If there's no endpoint configuration, then Kestrel binds to http://localhost:5000.

https://learn.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel/endpoints?view=aspnetcore-8.0

```cs
builder.WebHost.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Loopback, 5000);
    serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.ClientCertificateMode = ClientCertificateMode.AllowCertificate;
listenOptions.SslProtocols = SslProtocols.Tls13;
    });

});
```

# A Custom Middleware

```cs
// Add custom middleware for diagnostics

app.Use(async (context, next) => { 
    // Log the request 
    Console.WriteLine($"Request: {context.Request.Method} {context.Request.Path}"); 
    await next.Invoke(); // Call the next middleware     
    // Log the response 
    Console.WriteLine($"Response: {context.Response.StatusCode}"); 
});
```

# HttpLogging

```cs
services.AddHttpLogging(logging => 
    { logging.LoggingFields = HttpLoggingFields.RequestProperties | HttpLoggingFields.ResponseProperties; 
});
```

# HttpClient

```cs
        private async Task<List<UserData>> GetUserDatas()
        {
            using HttpClient httpClient = new() { BaseAddress = new Uri("http://localhost:7001") };
            HttpRequestMessage httpRequest = new(HttpMethod.Get, $"datas.json");
            HttpResponseMessage httpResponse = await httpClient.SendAsync(httpRequest);
            if (httpResponse != null && httpResponse.StatusCode == HttpStatusCode.OK)
            {
                return await httpResponse.Content.ReadFromJsonAsync<List<UserData>>();
            }
            throw new Exception("Boş");
        }
```

# Installer Service

```cs
builder.Services.InstallerServicesFromAssembly<Program>(config);

public static void InstallerServicesFromAssembly<T>(this IServiceCollection services, IConfiguration config)
{
var installers = typeof(T).Assembly.ExportedTypes.Where(x=> typeof(IInstaller).IsAssignableFrom(x) && x is {IsInterface: false, IsAbstract: false })
.Select(Activator.CreateInstance).Cast<IInstaller>().ToList();

installers.ForEach(installer => installer.InstallServices(services, config));
}
```

# Number Formats

```cs
String.Format("{0:C0}", Model.Data)

ar nfi = (NumberFormatInfo)CultureInfo.InvariantCulture.NumberFormat.Clone();

nfi.NumberGroupSeparator = " ";

string formatted = 1234897.11m.ToString("#,0.00", nfi); // "1 234 897.11"
```