# Create and Initialize Ubuntu Server

1. Create SSH keys

2. Create an Ubuntu server and add SSH public key.

3. SSH Connect to server and change SSH port to a custom port (other that 22) and disable password login.

4. Add the previous custom port to the firewall for allowing future SSH connections.

5. Reload firewall.

6. Reload SSH.

7. Quit and reconnect.

8. Update server.

```zsh
$ sudo apt update        # Fetches the list of available updates
$ sudo apt upgrade -y    # Installs some updates; does not remove packages
$ sudo apt full-upgrade  # Installs updates, handling dependencies; may also remove some packages, if needed
$ sudo apt autoremove    # Removes any old packages that are no longer needed
$ sudo apt autoclean     # 
```

9. Reboot Ubuntu Server.

```zsh
$ sudo reboot
```

10. Install .NET SDK or .NET Runtime on Ubuntu

(https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu-install?tabs=dotnet9&pivots=os-linux-ubuntu-2410)

Install the SDK (which includes the runtime) if you want to develop .NET apps. Or, if you only need to run apps, install the Runtime. If you're installing the Runtime, we suggest you install the ASP.NET Core Runtime as it includes both .NET and ASP.NET Core runtimes.

Use the `dotnet --list-sdks` and `dotnet --list-runtimes` commands to see which versions are installed.

> Ubuntu 24.04
> .NET is available in the Ubuntu .NET backports package repository. To add the repository, open a terminal and run the following command:
```zsh
$ sudo add-apt-repository ppa:dotnet/backports
```


```zsh
# To install the .NET SDK, run the following commands:
$ sudo apt-get update && sudo apt-get install -y dotnet-sdk-9.0
# The following commands install the ASP.NET Core Runtime
$ sudo apt-get update && sudo apt-get install -y aspnetcore-runtime-9.0
# As an alternative to the ASP.NET Core Runtime, you can install the .NET Runtime, which doesn't include ASP.NET Core support
$ udo apt-get install -y dotnet-runtime-9.0
```

When you install with a package manager, these libraries are installed for you. But, if you manually install .NET or you publish a self-contained app, you'll need to make sure these libraries are installed:

* ca-certificates
* libc6
* libgcc-s1
* libicu74
* liblttng-ust1
* libssl3
* libstdc++6
* libunwind8
* zlib1g

11. Install Nginx Server.

```zsh
$ sudo apt update
$ sudo apt install nginx

$ sudo service nginx start
$ sudo service nginx stop
$ sudo service nginx enable
$ sudo service nginx restart
```

Default page is placed in /var/www/html/ location. You can place your static pages here, or use virtual host and place it other location.

To set up virtual host, we need to create file in `/etc/nginx/sites-enabled` directory.

```json
server {
       listen 81;
       listen [::]:81;

       server_name example.ubuntu.com;

       root /var/www/tutorial;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
```



