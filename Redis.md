# Redis Stack

To get started with Redis Stack using Docker, you first need to select a Docker image: 

## redis/redis-stack 

Contains both Redis Stack server and Redis Insight. This container is best for local development because you can use the embedded Redis Insight to visualize your data. To start a Redis Stack container using the redis-stack image, run the following command in your terminal: 

```zsh 
% docker run -d --name redis-stack -p 6379:6379 -p 8001:8001 redis/redis-stack:latest 
``` 

## redis/redis-stack-server 

Provides Redis Stack server only. This container is best for production deployment. To start Redis Stack server using the redis-stack-server image, run the following command in your terminal: 

```zsh 
% docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest 
``` 

## Connect with redis-cli 

You can then connect to the server using redis-cli, just as you connect to any Redis instance. If you don’t have redis-cli installed locally, you can run it from the Docker container: 

```zsh 
% docker exec -it redis-stack redis-cli 
``` 

## Persistence in Docker 

To mount directories or files to your Docker container, specify -v to configure a local volume. This command stores all data in the local directory local-data: 

```zsh 
% docker run -v /local-data/:/data redis/redis-stack:latest 
``` 

## Ports 

If you want to expose Redis Stack server or Redis Insight on a different port, update the left hand of portion of the -p argument. This command exposes Redis Stack server on port 10001 and Redis Insight on port 13333: 

```zsh 
% docker run -p 10001:6379 -p 13333:8001 redis/redis-stack:latest 
``` 

## Config files 

By default, the Redis Stack Docker containers use internal configuration files for Redis. To start Redis with local configuration file, you can use the -v volume options: 
 
```zsh 
% docker run -v `pwd`/local-redis-stack.conf:/redis-stack.conf -p 6379:6379 -p 8001:8001 redis/redis-stack:latest 
``` 

## Environment variables 

To pass in arbitrary configuration changes, you can set any of these environment variables: 

* REDIS_ARGS: extra arguments for Redis 
* REDISEARCH_ARGS: arguments for the search and query features (RediSearch) 
* REDISJSON_ARGS: arguments for JSON (RedisJSON) 
* REDISTIMESERIES_ARGS: arguments for time series (RedisTimeSeries) 
* REDISBLOOM_ARGS: arguments for the probabilistic data structures (RedisBloom) 

```zsh 
# Pass a variable 
% docker run -e REDIS_ARGS="--requirepass redis-stack" redis/redis-stack:latest 

# Redis persistence: 
% docker run -e REDIS_ARGS="--save 60 1000 --appendonly yes" redis/redis-stack:latest 

# Retention policy for time series: 
% docker run -e REDISTIMESERIES_ARGS="RETENTION_POLICY=20" redis/redis-stack:latest 
``` 

# My Setup 

```zsh 
% docker run -d --name redis-stack -p 6379:6379 -p 8001:8001 -v /local-data/:/data redis/redis-stack:latest 
``` 

# Redis security 

Redis is designed to be accessed by trusted clients inside trusted environments. This means that usually it is not a good idea to expose the Redis instance directly to the internet or, in general, to an environment where untrusted clients can directly access the Redis TCP port or UNIX socket. 

Access to the Redis port should be denied to everybody but trusted clients in the network, so the servers running Redis should be directly accessible only by the computers implementing the application using Redis. 

In the common case of a single computer directly exposed to the internet, such as a virtualized Linux instance, the Redis port should be firewalled to prevent access from the outside. Clients will still be able to access Redis using the loopback interface. 

Note that it is possible to bind Redis to a single interface by adding a line like the following to the redis.conf file: 

``` text 
bind 127.0.0.1 
``` 

Failing to protect the Redis port from the outside can have a big security impact because of the nature of Redis. For instance, a single FLUSHALL command can be used by an external attacker to delete the whole data set. 

Since version 3.2.0, Redis enters a special mode called protected mode when it is executed with the default configuration (binding all the interfaces) and without any password in order to access it. In this mode, Redis only replies to queries from the loopback interfaces, and replies to clients connecting from other addresses with an error. We expect protected mode to seriously decrease the security issues caused by unprotected Redis instances executed without proper administration. However, the system administrator can still ignore the error given by Redis and disable protected mode or manually bind all the interfaces. 

## Authentication 

Redis provides two ways to authenticate clients. The recommended authentication method, introduced in Redis 6, is via Access Control Lists, allowing named users to be created and assigned fine-grained permissions. 

The legacy authentication method is enabled by editing the redis.conf file, and providing a database password using the requirepass setting. This password is then used by all clients. 

When the requirepass setting is enabled, Redis will refuse any query by unauthenticated clients. A client can authenticate itself by sending the AUTH command followed by the password. 

The password is set by the system administrator in clear text inside the redis.conf file. It should be long enough to prevent brute force attacks for two reasons: 

* Redis is very fast at serving queries. Many passwords per second can be tested by an external client. 
* The Redis password is stored in the redis.conf file and inside the client configuration. Since the system administrator does not need to remember it, the password can be very long. 

The goal of the authentication layer is to optionally provide a layer of redundancy. If firewalling or any other system implemented to protect Redis from external attackers fail, an external client will still not be able to access the Redis instance without knowledge of the authentication password. 

Since the AUTH command, like every other Redis command, is sent unencrypted, it does not protect against an attacker that has enough access to the network to perform eavesdropping. 

It is possible to disallow commands in Redis or to rename them as an unguessable name, so that normal clients are limited to a specified set of commands. 
 
``` zsh
# rename a command 
rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52 

# completely disallow command by renaming it to the empty string 
rename-command CONFIG "" 
``` 

### ACL 

The Redis ACL, short for Access Control List, is the feature that allows certain connections to be limited in terms of the commands that can be executed and the keys that can be accessed. The way it works is that, after connecting, a client is required to provide a username and a valid password to authenticate. If authentication succeeded, the connection is associated with a given user and the limits the user has. Redis can be configured so that new connections are already authenticated with a "default" user (this is the default configuration). Configuring the default user has, as a side effect, the ability to provide only a specific subset of functionalities to connections that are not explicitly authenticated. 

In the default configuration, Redis 6 (the first version to have ACLs) works exactly like older versions of Redis. Every new connection is capable of calling every possible command and accessing every key, so the ACL feature is backward compatible with old clients and applications. Also the old way to configure a password, using the requirepass configuration directive, still works as expected. However, it now sets a password for the default user. 

The Redis AUTH command was extended in Redis 6, so now it is possible to use it in the two-arguments form: 

``` 
AUTH <username> <password> 
``` 

Here's an example of the old form: 

``` 
AUTH <password> 
``` 

What happens is that the username used to authenticate is "default", so just specifying the password implies that we want to authenticate against the default user. This provides backward compatibility. 

``` 
ACL SETUSER alice 
ACL LIST 
``` 

# ASPNET Core Sample
 
```cs 
using System.Diagnostics; 
using Microsoft.AspNetCore.Mvc; 
using NRedisStack; 
using NRedisStack.RedisStackCommands; 
using rediss.Models; 
using StackExchange.Redis; 

// https://redis.io/docs/latest/develop/connect/clients/dotnet/ 
// dotnet add package NRedisStack 
namespace rediss.Controllers; 

public class RedisConnectorHelper 
{ 
    static RedisConnectorHelper() 
    { 
        RedisConnectorHelper.lazyConnection = new Lazy<ConnectionMultiplexer>(() => 
        { 
            return ConnectionMultiplexer.Connect("localhost"); 
        }); 
    } 

    private static Lazy<ConnectionMultiplexer> lazyConnection; 

    public static ConnectionMultiplexer Connection 
    { 
        get 
        { 
            return lazyConnection.Value; 
        } 
    } 

    /* 
    ConfigurationOptions options = new ConfigurationOptions 
    { 
        //list of available nodes of the cluster along with the endpoint port. 
            EndPoints = { 
        { "localhost", 16379 }, 
        { "localhost", 16380 }, 
        // ... 
        }, 
    }; 

    ConnectionMultiplexer cluster = ConnectionMultiplexer.Connect(options); 
    IDatabase db = cluster.GetDatabase(); 
    */ 

    /* 
    ConfigurationOptions options = new ConfigurationOptions 
    { 
        EndPoints = { { "my-redis.cloud.redislabs.com", 6379 } }, 
        User = "default",  // use your Redis user. More info https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/ 
        Password = "secret", // use your Redis password 
        Ssl = true, 
        SslProtocols = System.Security.Authentication.SslProtocols.Tls12                 
    }; 

    options.CertificateSelection += delegate 
    { 
        return new X509Certificate2("redis.pfx", "secret"); // use the password you specified for pfx file 
    }; 
    options.CertificateValidation += ValidateServerCertificate; 

    bool ValidateServerCertificate( 
            object sender, 
            X509Certificate? certificate, 
            X509Chain? chain, 
            SslPolicyErrors sslPolicyErrors) 
    { 
        if (certificate == null) { 
            return false;        
        } 

        var ca = new X509Certificate2("redis_ca.pem"); 
        bool verdict = (certificate.Issuer == ca.Subject); 
        if (verdict) { 
            return true; 
        } 
        Console.WriteLine("Certificate error: {0}", sslPolicyErrors); 
        return false; 
    } 

    ConnectionMultiplexer muxer = ConnectionMultiplexer.Connect(options);    

    //Creation of the connection to the DB 
    IDatabase conn = muxer.GetDatabase(); 

    //send SET command 
    conn.StringSet("foo", "bar"); 

    //send GET command and print the value 
    Console.WriteLine(conn.StringGet("foo"));    
    */ 
} 

public class HomeController : Controller 
{ 
    private readonly ILogger<HomeController> _logger; 

    public HomeController(ILogger<HomeController> logger) 
    { 
        _logger = logger; 
    } 

    [Obsolete] 
    public IActionResult Index() 
    { 

        // var cache = RedisConnectorHelper.Connection.GetDatabase(); 

        ConnectionMultiplexer redis = ConnectionMultiplexer.Connect("localhost"); 
        IDatabase db = redis.GetDatabase(); 

        db.StringSet("foo", "bar"); 
        Console.WriteLine(db.StringGet("foo")); // prints bar 

        var hash = new HashEntry[] { 
            new HashEntry("name", "John"), 
            new HashEntry("surname", "Smith"), 
            new HashEntry("company", "Redis"), 
            new HashEntry("age", "29"), 
            }; 
        db.HashSet("user-session:123", hash); 

         

        var hashFields = db.HashGetAll("user-session:123"); 
        Console.WriteLine(String.Join("; ", hashFields)); 
        // Prints: 
        // name: John; surname: Smith; company: Redis; age: 29 

        IBloomCommands bf = db.BF(); 
        ICuckooCommands cf = db.CF(); 
        ICmsCommands cms = db.CMS(); 
        IGraphCommands graph = db.GRAPH(); 
        ITopKCommands topk = db.TOPK(); 
        ITdigestCommands tdigest = db.TDIGEST(); 
        ISearchCommands ft = db.FT(); 
        IJsonCommands json = db.JSON(); 
        ITimeSeriesCommands ts = db.TS(); 

        return View(); 
    } 

    public IActionResult Privacy() 
    { 
        return View(); 
    } 

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)] 
    public IActionResult Error() 
    { 
        return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier }); 
    } 
}
``` 





















# References 

https://redis.io/docs/latest/operate/oss_and_stack/ 
https://redis.io/docs/latest/develop/connect/clients/dotnet/ 
https://redis.io/docs/latest/operate/oss_and_stack/management/security/ 

