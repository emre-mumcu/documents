# Publishing and hosting a .NET Core MVC application on Ubuntu Linux

## 1. Publish the Application

The default build configuration is Release, which is appropriate for a deployed site running in production. The output from the Release build configuration has minimal symbolic debug information and is fully optimized.

1. Start Visual Studio Code. 
2. Choose View > Terminal from the main menu.
3. Run the following command in your terminal to publish the application:

> dotnet publish --configuration Release --output ./publish

This generates the compiled application files in the specified ./publish directory.

## 2. Install .NET Core Runtime on Ubuntu

Update your package index and install dependencies:

> sudo apt update
> sudo apt install -y wget apt-transport-https

Add the Microsoft package repository:

> wget https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
> sudo dpkg -i packages-microsoft-prod.deb

Install the .NET Runtime

> sudo apt update
> sudo apt install -y aspnetcore-runtime-9.0

## 3. Configure the Application

Run the Application Manually:

> dotnet myapp.dll

By default, the application listens on http://localhost:5000 or http://localhost:5001 for HTTPS.  

Verify it works locally by accessing the URL using curl or your browser.

## 4. Set Up a Reverse Proxy with Nginx




# 2.4. Setup Dotnet & Kestrel Service

**Dotnet Install**

dnf install -y dotnet-runtime-7.0
dnf install -y aspnetcore-runtime-7.0
dnf install -y dotnet-runtime-7.0

**Kestrel Service**

```bash
vi /etc/systemd/system/kestrel-myapp.service
```

```
[Unit]
Description=My application description
 
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
systemctl enable myapp.service
systemctl daemon-reload
systemctl reset-failed
systemctl start myapp.service
systemctl status myapp.service

systemctl list-units kestrel-* --all
systemctl list-unit-files kestrel-*
systemctl status kestrel-*
```

**ASP.NET Application Configuration**

```csharp
services.Configure<ForwardedHeadersOptions>(options => {
  options.ForwardedHeaders = 
    ForwardedHeaders.XForwardedFor | 
    ForwardedHeaders.XForwardedProto | 
    ForwardedHeaders.XForwardedHost;
});
  
app.UseRouting(); 
app.UseForwardedHeaders(); 
app.UseAuthentication();
```

**Nginx Configuration**

```json
server {
    listen        80;
    server_name   example.com www.example.com;
    location / {
        proxy_pass         http://localhost:5000;
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




Install Nginx

> sudo apt install nginx

Create a new configuration file for your application

> sudo nano /etc/nginx/sites-available/myapp

Add the following configuration

```text
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Enable the configuration

> sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/

Test and restart Nginx:

> sudo nginx -t
> sudo systemctl restart nginx

## 5. 5. Run the Application as a Service

Create a systemd service file:

> sudo nano /etc/systemd/system/myapp.service

Add the following content:

```text
[Unit]
Description=My .NET Core MVC Application
After=network.target

[Service]
WorkingDirectory=/var/www/myapp
ExecStart=/usr/bin/dotnet /var/www/myapp/myapp.dll
Restart=always
RestartSec=10
SyslogIdentifier=myapp
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target

```

Reload the systemd daemon:

> sudo systemctl daemon-reload

Start and enable the service:

> sudo systemctl start myapp
> sudo systemctl enable myapp

Check the status

> sudo systemctl status myapp

## 6. Configure Firewall (Optional)

If you have a firewall enabled, allow Nginx:

> sudo ufw allow 'Nginx Full'



