# UFW Essentials

UFW (uncomplicated firewall) is a firewall configuration tool that runs on top of iptables, included by default within Ubuntu distributions. It provides a streamlined interface for configuring common firewall use cases via the command line.

To check if ufw is enabled, run:

```zsh
$ sudo ufw status
$ sudo ufw disable
$ sudo ufw enable
```

## Allow an IP Address

```zsh
$ sudo ufw allow from 203.0.113.101
```

## Allow Incoming Connections to a Network Interface

```zsh
$ sudo ufw allow in on eth0 from 203.0.113.102
```

## Block IP Address

```zsh
$ sudo ufw deny from 203.0.113.100
```

## Block a Subnet

```zsh
$ sudo ufw deny from 203.0.113.0/24
```

## Block Incoming Connections to a Network Interface

```zsh
$ sudo ufw deny in on eth0 from 203.0.113.100
```

The in parameter tells ufw to apply the rule only for incoming connections, and the on eth0 parameter specifies that the rule applies only for the eth0 interface.

## Delete UFW Rule

To delete a rule that you previously set up within UFW, use ufw delete followed by the rule (allow or deny) and the target specification. Another way to specify which rule you want to delete is by providing the rule ID. This information can be obtained with the following command:

```zsh
$ sudo ufw status numbered
```

To delete a rule by its ID, run:

```zsh
$ sudo ufw delete 1
```

## List Available Application Profiles

Upon installation, applications that rely on network communications will typically set up a UFW profile that you can use to allow connection from external addresses. This is often the same as running ufw allow from, with the advantage of providing a shortcut that abstracts the specific port numbers a service uses and provides a user-friendly nomenclature to referenced services.

To list which profiles are currently available, run the following:

```zsh
$ sudo ufw app list
```

If you installed a service such as a web server or other network-dependent software and a profile was not made available within UFW, first make sure the service is enabled. For remote servers, you’ll typically have OpenSSH readily available.

To enable a UFW application profile, run ufw allow followed by the name of the application profile you want to enable, which you can obtain with a sudo ufw app list command.

```zsh
$ sudo ufw allow "OpenSSH"
```

To allow incoming connections from a specific IP address or subnet, you’ll include a from directive to define the source of the connection. This will require that you also specify the destination address with a to parameter. To lock this rule to SSH only, you’ll limit the proto (protocol) to tcp and then use the port parameter and set it to 22, SSH’s default port.

```zsh
$ sudo ufw allow from 203.0.113.103 proto tcp to any port 22
```

To disable an application profile that you had previously set up within UFW, you’ll need to remove its corresponding rule.

```zsh
$ sudo ufw allow "Nginx HTTPS"
$ sudo ufw delete allow "Nginx Full"
```

## Allow Nginx HTTP / HTTPS

```zsh
$ sudo ufw app list | grep Nginx
# To enable both HTTP and HTTPS traffic, choose Nginx Full. Otherwise, choose either Nginx HTTP to allow only HTTP or Nginx HTTPS to allow only HTTPS.
$ sudo ufw allow "Nginx Full"
```

## Allow All Incoming HTTP

Web servers, such as Apache and Nginx, typically listen for HTTP requests on port 80. If your default policy for incoming traffic is set to drop or deny, you’ll need to create a UFW rule to allow external access on port 80. You can use either the port number or the service name (http) as a parameter to this command.

To allow all incoming HTTP (port 80) connections, run:

```zsh
# http
$ sudo ufw allow http
# or
$ sudo ufw allow 80

# https
$ sudo ufw allow https
# or
$ sudo ufw allow 443
```

If you want to allow both HTTP and HTTPS traffic, you can create a single rule that allows both ports. 

```zsh
$ sudo ufw allow proto tcp from any to any port 80,443
```

# References

* https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands