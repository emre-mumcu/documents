# Using HTTPS with Kestrel in .NET Core

appsettings.Development.json

```json
{
	"Logging": {
		"LogLevel": {
			"Default": "Information",
			"Microsoft.AspNetCore": "Warning"
		}
	},
	"Kestrel1": {
		"Certificates": {
			"Default": {
				"Path": "localhost.pfx",
				"Password": "aA123456"
			}
		}
	},
	"Kestrel": {
		"Endpoints": {
			"Http": {
				"Url": "http://*:5000"
			},
			"Https": {
				"Url": "https://*:5001",
				"Certificate": {
					"Path": "localhost.pfx",
					"KeyPassword": "aA123456"
				}
			}
		},
		"Limits": {
			"MaxConcurrentConnections": 100,
			"MaxRequestBodySize": 10485760
		}
	}
}
```

# Program.cs

```cs
var builder = WebApplication.CreateBuilder(args);

bool useAppsettings = true;

if (!useAppsettings)
{
	// Configure Kestrel 
	builder.WebHost.ConfigureKestrel(options =>
	{
		// Listen on port 5000 for HTTP
		options.ListenAnyIP(5000);

		// Listen on port 5001 for HTTPS
		options.ListenAnyIP(5001, listenOptions =>
		{

			// Use default dotnet development certificate
			// listenOptions.UseHttps();

			// Use custom certificate
			var pfxFilePath = "localhost.pfx";
			var pfxPassword = "aA123456";
			listenOptions.UseHttps(pfxFilePath, pfxPassword);
		});

		options.Limits.MaxConcurrentConnections = 100;

		options.Limits.MaxRequestBodySize = 10 * 1024; // 10 KB
	});
}
else
{
	// Configure Kestrel using appsettings.json
	builder.WebHost.ConfigureKestrel((context, options) =>
	{
		var kestrelConfig = context.Configuration.GetSection("Kestrel");
		options.Configure(kestrelConfig);
	});
}
```