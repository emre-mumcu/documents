# Publishing and hosting an ASP.NET Core MVC application on Ubuntu with Nginx Reverse Proxy

## 1. Configure the Application

Kestrel is great for serving dynamic content from ASP.NET Core. However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx. A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and HTTPS termination from the HTTP server.

ASP.NET Core MVC application will run behind a single instance Nginx reverse proxy, which runs on the same server, alongside the HTTP server (Kestrel). 

Since the application will not be serving http requests directly, we will not use HTTPS redirection and add a configuration for Kestrel:

```cs
#if !DEBUG
    builder.WebHost.ConfigureKestrel((context, serverOptions) =>
    {
        // serverOptions.ListenAnyIP(5055, listenOptions => { listenOptions.UseHttps(); });
        serverOptions.ListenAnyIP(5055);
    });

#endif
```

Because requests will be forwarded by Nginx, we will configuree the Forwarded Headers in application so that redirect URIs and other security policies work correctly.

Forwarded Headers Middleware should run before other middleware. This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.

```cs
var app = builder.Build();

app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | 
    ForwardedHeaders.XForwardedProto | 
    ForwardedHeaders.XForwardedHost
});

services.Configure<ForwardedHeadersOptions>(options => {
  options.ForwardedHeaders = 
    ForwardedHeaders.XForwardedFor | 
    ForwardedHeaders.XForwardedProto | 
    ForwardedHeaders.XForwardedHost;
});
```

If no ForwardedHeadersOptions are specified to the middleware, the default headers to forward are None.

Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default. If other trusted proxies or networks within the organization handle requests between the internet and the web server, add them to the list of KnownProxies or KnownNetworks with ForwardedHeadersOptions. 

```cs
var builder = WebApplication.CreateBuilder(args);

// Configure forwarded headers
builder.Services.Configure<ForwardedHeadersOptions>(options =>
{
    // options.ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto | ForwardedHeaders.XForwardedHost;
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});

var app = builder.Build();

// app.UseForwardedHeaders(); 
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});
```

## 2. Publish the Application

1. Start Visual Studio Code. 
2. Open Terminal. (View > Terminal)
3. Run the following command in your terminal to publish the application:

```
dotnet publish
```

Publish will be placed at `bin/Release/netX.X` folder by default.

The default build configuration is Release, which is appropriate for a deployed site running in production. The output from the Release build configuration has minimal symbolic debug information and is fully optimized.

By default, the publishing process creates a framework-dependent deployment, which is a type of deployment where the published application runs on a machine that has the .NET runtime installed. To run the published app you can use the *executable* file or run the `dotnet MyApp.dll` command from a command prompt.

* `MyApp.dll` This is the framework-dependent deployment version of the application. To run this dynamic link library, enter `dotnet MyApp.dll` at a command prompt. This method of running the app works on any platform that has the .NET runtime installed.

* `MyApp.exe` (MyApp on Linux or macOS) This is the framework-dependent executable version of the application. The file is operating-system-specific. You can run the app by using the executable. On Windows, enter `.\MyApp.exe` and press Enter. On Linux, enter `./MyApp` and press Enter.

## 3. Install .NET Core Runtime

Install the SDK (which includes the runtime) if you want to develop .NET apps. Or, if you only need to run apps, install the Runtime. If you're installing the Runtime, we suggest you install the ASP.NET Core Runtime as it includes both .NET and ASP.NET Core runtimes.

.NET is available in the Ubuntu package manager feeds. The Microsoft package repository no longer contains .NET packages for Ubuntu.

```
To install the .NET SDK, run the following commands:
sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-9.0

To install the ASP.NET Core Runtime, run the following commands:
sudo apt-get update && \
  sudo apt-get install -y aspnetcore-runtime-9.0

To install the .NET Runtime, run the following commands:  
sudo apt-get install -y dotnet-runtime-9.0
```

You can use the following commands to see which versions are installed.

```
dotnet --list-sdks 
dotnet --list-runtimes 
```

## 4. Install Nginx

Use apt-get to install Nginx. The installer creates a systemd init script that runs Nginx as daemon on system startup. 

```shell
# install nginx
sudo apt install nginx

# enable nginx service
sudo systemctl enable nginx

# start nginx
sudo service nginx status
sudo service nginx start
```

## 5. Create Kestrel Service

Create a new systemd service file for your application.

```
vi /etc/systemd/system/kestrel-myapp.service
```

```
[Unit]
Description=My application description
After=network.target

[Service]
WorkingDirectory=/inetpub/myapp
ExecStart=/usr/lib64/dotnet/dotnet /inetpub/myapp/myapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=myapp
User=kestrel
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false
 
[Install]
WantedBy=multi-user.target
```

```bash
systemctl reset-failed
systemctl daemon-reload

systemctl enable myapp.service
systemctl start myapp
systemctl status myapp

systemctl list-units kestrel-* --all
systemctl list-unit-files kestrel-*
systemctl status kestrel-*
```


## 7. Configure Nginx

Create a new configuration file for your application.

Create a `/etc/nginx/sites-available/myapp` file with a text editor, and replace the contents with the following snippet:

```
server {
    listen 80;
    server_name mumcu.net www.mumcu.net;
    location / {
        proxy_pass         http://127.0.0.1:5000/;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```


After creating the configuration file, use the following command to create the symlink to enable the configuration:
```
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
```

Test and restart Nginx:
```
sudo nginx -t
sudo systemctl restart nginx
```

When no server_name matches, Nginx uses the default server.  If no default server is defined, the first server in the configuration file is the default server. As a best practice, add a specific default server that returns a status code of 444 in your configuration file. 

```
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

## 6. Configure Firewall (Optional)

If you have a firewall enabled, allow Nginx:

```
sudo ufw allow 'Nginx Full'
```


## 7. Transfer Application File

You can use ssh in combination with scp (secure copy protocol) to transfer files to a remote server.

Create a zip file (MyApp.zip) from the published application files.

```
scp /path/to/local/file username@remote_host:/path/to/remote/destination
scp -P port_number /path/to/local/file username@remote_host:/path/to/remote/destination
scp myfile.txt user@192.168.1.100:/home/user/

Transfer an entire directory: Add the -r flag to copy directories recursively.
scp -r /path/to/local/directory username@remote_host:/path/to/remote/destination

scp -P XXXXX "C:\Users\emumc\Desktop\emremumcu.zip" root@1.1.1.1:/temp

temp# unzip emremumcu.zip -d /inetpub/

```





