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

# 1. Initial Server Setup

When you first create a new Rocky Linux server, there are a few configuration steps that you should take early on as part of the basic setup.

```bash
ssh root@server_ip
```
Once login successfully, Change the default root password with a strong one:

```bash
passwd
```

## 1.1. Setting Up and Configuring a Basic Firewall

To get private IP:

```bash
hostname -I
ifconfig -a
ip addr (ip a)
hostname -I | awk '{print $1}'
ip route get 1.2.3.4 | awk '{print $7}'
(Fedora) Wifi-Settings→ click the setting icon next to the Wifi name that you are connected to → Ipv4 and Ipv6 both can be seen
nmcli -p device show
```

To get public IP:

```bash
curl ifconfig.me
curl -4/-6 icanhazip.com
curl ipinfo.io/ip
curl api.ipify.org
curl checkip.dyndns.org
dig +short myip.opendns.com @resolver1.opendns.com
host myip.opendns.com resolver1.opendns.com
curl ident.me
curl bot.whatismyipaddress.com
curl ipecho.net/plain
```

Firewalls provide a basic level of security for your server. If a firewall has not been installed yet:

**Firewall Setup**

```bash
dnf install -y firewalld
systemctl start firewalld
systemctl status firewalld
systemctl enable firewalld
```

**Firewall Configuration**

```bash
# Query Firewall
firewall-cmd --state
firewall-cmd --get-services
firewall-cmd --get-active-zones
firewall-cmd --permanent --list-all
firewall-cmd --zone=public --list-services
firewall-cmd --zone=public --list-ports
# Configuration
firewall-cmd --permanent --zone=public --add-service=http 
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --zone=public --permanent --add-port=xxxx/tcp
firewall-cmd --permanent --service="xxx" --add-port "xxxx/tcp"
```

# 1.2. SSH Configuration

By default, SSH is configured to run using port 22. We will change this to another port to avoid bot attacks to our server.

**Firewall configuration for the new SSH port**

```bash
# Configure the firewall to allow new SSH port:
# Note: Ports which are equal or lower than 1024 are reserved for admin use
firewall-cmd --permanent --add-service=ssh
firewall-cmd --zone=public --permanent --add-port=11977/tcp
firewall-cmd --permanent --service="ssh" --add-port "11977/tcp"
firewall-cmd --reload 
firewall-cmd --zone=public --list-services
firewall-cmd --zone=public --list-ports
```

**SELINUX configuration for SSH**

```bash
sestatus
getenforce
setenforce 0

vi /etc/selinux/config
# SELINUX=permissive
```

**SSH COnfiguration for a new Port**

```bash
vi /etc/ssh/sshd_config
# Port 11977
# ClientAliveInterval 15m
# ClientAliveCountMax 4 
# PermitRootLogin yes

systemctl restart sshd.service
firewall-cmd --permanent --service="ssh" --remove-port "22/tcp"
firewall-cmd --reload
```
Reconnect to SSH using new port:

```bash
ssh -p 11977 user@server_ip
```

## Updates

Keep your server up to date, by running this command.

```bash
dnf update -y
```

## 1.3. Create a User with SUDO Privileges (with login)

```bash
# Create user
adduser emre
# Set password
passwd emre
# Add to wheel group. Users who belong to the wheel group are allowed to use the sudo command
usermod -aG wheel emre
```

## 1.4. Create a system user (with no login)

```bash
# Use one of the following commands to create a kestrel user without login
useradd -s /usr/sbin/nologin -r -M -d /dev/null kestrel 
useradd -s /bin/false -r kestrel
useradd -s /usr/sbin/nologin -r -M kestrel
```

## 1.5. Update the System

```bash
dnf update
```

# 2. Install LEMP Stack

The terms MEAN, MERN, LEMP, PERN, etc are web stacks consisting of a bundle of software and frameworks or libraries which are used for building full-stack web apps. A stack usually consists of a database, server-side and client-side technologies, a web server, a particular Operating system.

LEMP is an open-source web application stack used to develop web applications. The term LEMP is an acronym that represents L for the Linux Operating system, Nginx (pronounced as engine-x, hence the E in the acronym) web server, M for MySQL database, and P for PHP scripting language.

# 2.1. Install Nginx Web Server

curl -4 icanhazip.com

Nginx is a lightweight application that can be used as either a web server or reverse proxy. You should have a regular, non-root user with sudo privileges configured on your server.

```bash
sudo dnf install -y nginx

sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx

sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx

sudo apt update
sudo apt install nginx



# Get Version
nginx -v
# Validate Configuration
nginx -t
```

To make your pages available to the public, you will have to edit your firewall rules to allow HTTP requests on your web server by using the following commands.

```bash
firewall-cmd --permanent --zone=public --add-service=http 
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload

sudo ufw app list
sudo ufw allow 'Nginx HTTP'

sudo ufw status
sudo ufw reload

systemctl status nginx
```

We need to make the user Nginx the owner of the web directory. By default, it's owned by the root user or the user that perform dnf install.

```bash
chown nginx:nginx /usr/share/nginx/html -R
```

Verify that the web server is running and accessible by accessing your server's IP address:

```bash
curl -4 serverip
# If you do not know your server's IP address, you can find it by using the icanhazip.com tool, which will give you your public IP address as received from another location on the internet:
$ curl -4 icanhazip.com
```

**Nginx Configuration**

* `/etc/nginx`: The Nginx configuration directory. All of the Nginx configuration files reside here.
* `/etc/nginx/nginx.conf`: The main Nginx configuration file. This can be modified to make changes to the Nginx global configuration.
* `/etc/nginx/conf.d/`: This directory contains server block configuration files, where you can define the websites that are hosted within Nginx. A typical approach is to have each website in a separate file that is named after the website's domain name, such as your_domain.conf.

#### Server Logs
* `/var/log/nginx/access.log`: Every request to your web server is recorded in this log file unless Nginx is configured to do otherwise.
* `/var/log/nginx/error.log`: Any Nginx errors will be recorded in this log.

**Setting Up Server Blocks**

When using the Nginx web server, server blocks (similar to virtual hosts in Apache) can be used to organize configuration details and host more than one domain from a single server.

On Rocky Linux, server blocks are defined in `.conf` files located at `/etc/nginx/conf.d` folder.

By default, Nginx on Rocky Linux is configured to serve documents out of a directory at `/usr/share/nginx/html`. While this works well for a single site, it can become unmanageable if you are hosting multiple sites. 

Instead of modifying `/usr/share/nginx/html`, you'll create a directory structure within `/inertpub/your_domain/` folder for the your_domain website, leaving `/usr/share/nginx/html` in place as the default directory to be served if a client request doesn't match any other sites.

```bash
# Create the directory for your_domain as follows, using the -p flag to create any necessary parent directories:
sudo mkdir -p /inetpub/mumcu.net/

# Next, assign ownership of the directory with the $USER environment variable, which should reference your current system user:
sudo chown -R $USER:$USER /inetpub/mumcu.net/

# Next, create a sample index.html page using your favorite editor and add the following content in it:
vi /inetpub/mumcu.net/index.html
```
```html
<html>
    <head>
        <title>Welcome to your_domain</title>
    </head>
    <body>
        <h1>Success! Your Nginx server is successfully configured for <em>your_domain</em>. </h1>
<p>This is a sample page.</p>
    </body>
</html>
```

In order for Nginx to serve this content, you'll need to create a server block with directives that point to your custom web root. Create a new server block at `/etc/nginx/conf.d/mumcu.net.conf`:

```bash
vi /etc/nginx/conf.d/mumcu.net.conf
```
```json
server {
        listen 80;
        listen [::]:80;
        root /inetpub/your_domain/;
        index index.html index.htm;
        server_name your_domain www.your_domain;
        location / {
                try_files $uri $uri/ =404;
        }
}
// or
server {
    listen       80;
    server_name  mumcu.net;
    location / {
        root   /inetpub/mumcu.net/;
        index  index.html;
    }
}
```

**www to domain redirection**

```bash
vi /etc/nginx/conf.d/www.mumcu.net.conf
```
```json
server {
    server_name www.mumcu.net;
    return 301 $scheme://mumcu.net$request_uri;
}
```

```bash
# Test to make sure that there are no syntax errors in any of your Nginx files:
sudo nginx -t
# If there aren't any problems, restart Nginx to enable your changes:
sudo systemctl restart nginx
```

Before you can test the changes from your browser, you'll need to update your server's SELinux security contexts so that Nginx is allowed to serve content from the `/inetpub/your_domain` directory. (If Selinux is enabled). This chcon context update will allow your custom document root to be served as HTTP content:

```bash
chcon -vR system_u:object_r:httpd_sys_content_t:s0 /inetpub/your_domain/
```

## Setting Up Server Blocks

When using the Nginx web server, server blocks (similar to virtual hosts in Apache) can be used to encapsulate configuration details and host more than one domain from a single server.

Nginx on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become unwieldy if you are hosting multiple sites. Instead of modifying /var/www/html, let’s create a directory structure within /var/www for our your_domain site, leaving /var/www/html in place as the default directory to be served if a client request doesn’t match any other sites.

Create the directory for your_domain as follows, using the -p flag to create any necessary parent directories:

> sudo mkdir -p /var/www/your_domain/html

Next, assign ownership of the directory with the $USER environment variable:

> sudo chown -R $USER:$USER /var/www/your_domain/html

The permissions of your web roots should be correct if you haven’t modified your umask value, which sets default file permissions. To ensure that your permissions are correct and allow the owner to read, write, and execute the files while granting only read and execute permissions to groups and others, you can input the following command:

> sudo chmod -R 755 /var/www/your_domain

Next, create a sample index.html page using nano or your favorite editor:

> sudo nano /var/www/your_domain/html/index.html

Save and close the file by pressing Ctrl+X to exit, then when prompted to save, Y and then Enter.

In order for Nginx to serve this content, it’s necessary to create a server block with the correct directives. Instead of modifying the default configuration file directly, let’s make a new one at /etc/nginx/sites-available/your_domain:

> sudo nano /etc/nginx/sites-available/your_domain

Paste in the following configuration block, which is similar to the default, but updated for our new directory and domain name:

```json
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Next, let’s enable the file by creating a link from it to the sites-enabled directory, which Nginx reads from during startup:

> sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/

sudo nginx -t
sudo systemctl restart nginx

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04

# 2.2. Install MariaDB Server

MariaDB is a popular database server.

```bash
dnf install -y mariadb-server mariadb

systemctl start mariadb
systemctl enable mariadb
systemctl status mariadb
systemctl restart mariadb

firewall-cmd --permanent --add-service=mysql
firewall-cmd --permanent --add-port=3306/tcp
```

Finally, you will want to secure your MariaDB installation by issuing the following command.

```bash
mysql_secure_installation
```

Once secured, you can connect to MySQL and review the existing databases on your database server by using the following command.

```bash
mysql -e "SHOW DATABASES;" [-p]
```

MariaDB configuration file:

```bash
vi /etc/my.cnf.d/mariadb-server.cnf
```

## Create Wordpress Users

Connect to MariaDB:

```bash
mysql -uroot
```

Run the following sql:

```sql
CREATE DATABASE wordpress;
-- Allows the connection from any IP
CREATE USER `wpadmin`@`%` IDENTIFIED BY 'aA123456';
GRANT ALL ON wordpress.* TO `wpadmin`@`%`;

-- Allows the connection from localhost only
CREATE USER `wpadmin`@`localhost` IDENTIFIED BY 'aA123456';
GRANT ALL ON wordpress.* TO `wpadmin`@`localhost`;

FLUSH PRIVILEGES;
exit;
```

# 2.3. Install PHP

Nginx doesn’t know how to run a PHP script of its own. It needs a PHP module like PHP-FPM to efficiently manage PHP scripts. PHP-FPM, on the other hand, runs outside the NGINX environment by creating its own process. Therefore when a user requests a PHP page the nginx server will pass the request to PHP-FPM service using FastCGI.

To Install PHP-FPM by running the following command.

```bash
dnf install php php-mysqlnd php-fpm php-opcache php-gd php-xml php-mbstring -y
# or
dnf install -y php php-zip php-intl php-mysqlnd php-dom php-simplexml php-xml php-xmlreader php-curl php-exif php-ftp php-gd php-iconv php-json php-mbstring php-posix php-sockets php-tokenizer
```

Once the installation is complete, enable php-fpm (to start automatically upon system boot), start the php-fpm, and verify the status using the commands below.

```bash
systemctl start php-fpm
systemctl enable php-fpm
systemctl status php-fpm
```

By default, PHP-FPM runs as the apache user. Since we are using the Nginx web server, we need to change the following line. Edit the file `/etc/php-fpm.d/www.conf` file.

```bash
vi /etc/php-fpm.d/www.conf
```

Find the below lines:
user = apache
group = apache

Change them into:
user = nginx
group = nginx

Once changed, you will need to reload php-fpm:

```bash
systemctl reload php-fpm
```

### Nginx-PHP Config

Add the following location block in the server section of `/etc/nginx/nginx.conf` file

```bash
    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;

# add this section start: 
    location ~* \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/run/php-fpm/www.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
# add this section end:

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }

```

restart nginx:
```bash
nginx -t
systemctl restart nginx
```

Test your PHP, by creating a simple info.php file with a phpinfo() in it. The file should be placed in the root directory for your web server, which is /usr/share/nginx/html/.

To create the file use:

```bash
echo "<?php phpinfo() ?>" > /usr/share/nginx/html/info.php
```

Restart the Nginx and PHP-FPM.
```bash
systemctl restart nginx php-fpm
```

**File Upload Size Limits**

By default, PHP file upload size is set to maximum 2MB file on the server. To increate this limit:

```bash
vi /etc/php.ini

# 855
upload_max_filesize = 50M
# 703
post_max_size = 50M
# 858 (maximum number of files allowed to be uploaded simultaneously)
max_file_uploads = 25
```

In Nginx add the following line to the nginx.conf:

```bash
vi /etc/nginx/nginx.conf

# add the below setting before server block after the include /etc/nginx/conf.d/*.conf; line: 
client_max_body_size 50M;

nginx -t
systemctl restart nginx php-fpm
```

Now again, access http://localhost/info.php or http://yourserver-ip-address/info.php. You should see an information page.

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

# 2.5. CERTBOT Setup

## 2.5.1. Ubuntu (Debian)

You'll need to install snapd if it is not already installed.

To install Certbot:
sudo snap install --classic certbot

Execute the following instruction on the command line on the machine to ensure that the certbot command can be run:
sudo ln -s /snap/bin/certbot /usr/bin/certbot

Run this command to get a certificate and have Certbot edit your nginx configuration automatically to serve it, turning on HTTPS access in a single step.
sudo certbot --nginx

If you're feeling more conservative and would like to make the changes to your nginx configuration by hand, run this command.
sudo certbot certonly --nginx

The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire. You will not need to run Certbot again, unless you change your configuration. You can test automatic renewal for your certificates by running this command:
sudo certbot renew --dry-run

## 2.5.2. Rocky Linux (RedHat)

```bash
dnf install epel-release mod_ssl -y
dnf install -y certbot python3-certbot-nginx

# Configure For All Sites
certbot --nginx
# Configure For Specific Sİtes
certbot --nginx -d website.com
certbot --nginx -d website.com -d http://www.website2.com
```

Set up automatic renewal running the following line, which will add a cron job to the default crontab. 
```bash
$ echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew" | sudo tee -a /etc/crontab > /dev/null 
```

If you want to check that your website certificate is valid, you can head to https://www.ssllabs.com/ssltest/ and check your website parameters.

To Update Certificates Manually:
```bash
$ certbot renew --dry-run
```


# 2.6. Create a Self-Signed SSL Certificate

```bash
$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
```

# 2.7. Wordpress Setup and Configuration

WordPress is a powerful and feature-rich opensource content management system (CMS) that allows users to create powerful and stunningly beautiful websites. It’s written in PHP and powered by MariaDB or MySQL database server at the backend. 

As a requirement, you need to have the LAMP stack installed on Rocky Linux 9.

**Creating the New Database**

```bash
# with password
mysql -u root -p

# without password
sudo mysql -u root
sudo mysql -uroot
```

```sql
-- Create a new database
CREATE DATABASE wordpress;
-- create a new MySQL user account
CREATE USER `wpadmin`@`localhost` IDENTIFIED BY '(FAbs4xu&TRob*=-#vMbCx]29q{jyf!c';
--Link the user and DB together by granting our user access to the database
GRANT ALL ON wordpress.* TO `wpadmin`@`localhost`;
-- Flush the privileges so that MySQL knows about the user permissions we just added
FLUSH PRIVILEGES;
--Exit out of the MySQL command prompt
exit;
```

**Configure Nginx For Wordpress**

We will create a root directory and provide this information to the Nginx server block.

```bash
mkdir -p /inetpub/emremumcu.com/
```

Next, create an Nginx configuration file for your website

```bash
vi /etc/nginx/conf.d/emremumcu.com.conf
```
``` json
server {
    listen         80;
    server_name    emremumcu.com;
    root           /inetpub/emremumcu.com/;
    index          index.html;

    location / {
      index index.php index.html index.htm;
      # Prevent 404 when permalinks change
      try_files $uri $uri/ /index.php?q=$uri$args;
      #If your root wordpress is not the webroot but http://domain.com/wordpress/
      #try_files $uri $uri/ /wordpress/index.php?q=$uri$args; 
      #try_files $uri $uri/ =404;
    }
    location ~* \.php$ {
      fastcgi_pass unix:/run/php-fpm/www.sock;
      include         fastcgi_params;
      fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
      fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
    }
}
``` 
www to domain redirection:

```bash
vi /etc/nginx/conf.d/www.emremumcu.com.conf
```
```json
server {
    server_name www.emremumcu.com;
    return 301 $scheme://emremumcu.com$request_uri;
}
```

Test the nginx configuration and restart the server:

```bash
nginx -t
systemctl restart nginx
```

**Wordpress Setup**

Download the WordPress and extract the downloaded file to the domain folder

```bash
# Using CURL:
mkdir -p /temp
cd /temp
curl -L -O http://wordpress.org/latest.tar.gz
tar xf latest.tar.gz
mv wordpress/* /inetpub/emremumcu.com/


# using WGET (not used)
dnf install -y wget unzip
wget https://wordpress.org/latest.zip
unzip latest.zip
mv wordpress /var/www/html/
chown -R apache:apache /var/www/html/wordpress
chmod -R 775 /var/www/html/wordpress
# Configure the SELinux context for the directory and its contents
semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/html/wordpress(/.*)?"
restorecon -Rv /var/www/html/wordpress
```

Next, we will configure the wp-config.php

```bash
cd /inetpub/emremumcu.com/
cp wp-config-sample.php wp-config.php
vi wp-config.php
```

Replace the following contents with the correct values

``` json
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );
/** Database username */
define( 'DB_USER', 'wpadmin' );
/** Database password */
define( 'DB_PASSWORD', '@cF0O,WXxCLO@mX%)WNRbSStH-5jsULE' );
```

Update file permissions

```bash
chown -R nginx:nginx /inetpub/emremumcu.com/
chcon -R -t httpd_sys_content_t /inetpub/emremumcu.com/
```

# EOF

