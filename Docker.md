# Rocky Linux

```zsh
% docker run -it --name rlinux -d rockylinux/rockylinux
% docker exec -it --user root rlinux /bin/bash
% dnf install httpd -y
# Start httpd
% httpd
# Check to see the server is running: 
% curl localhost
```

# Redis

```zsh
% docker run -d --restart=always --name Redis-Stack -p 6379:6379 -p 8001:8001 redis/redis-stack:latest
```

# SQLServer

The following script is for Microsoft PowerShell:

```shell
docker run ` 
     -e 'ACCEPT_EULA=Y' ` 
     -e 'MSSQL_SA_PASSWORD=aA123456' ` 
     -e 'MSSQL_PID=Developer' ` 
     -e 'MSSQL_COLLATION=Turkish_CI_AS' ` 
     -e 'MSSQL_TCP_PORT=1433' ` 
     -p 14333:1433 ` 
     --name mssqlserver ` 
     --hostname mssqlserver ` 
     --restart unless-stopped ` 
     -v c:\docker\sql\data:/var/opt/mssql/data ` 
     -v c:\docker\sql\log:/var/opt/mssql/log ` 
     -v c:\docker\sql\backup:/var/opt/mssql/backup ` 
     -v c:\docker\sql\secrets:/var/opt/mssql/secrets ` 
     -d mcr.microsoft.com/mssql/server:2019-latest
```

The following script is for MacOS Terminal:

```zsh
docker run \ 
     -e 'ACCEPT_EULA=Y' \ 
     -e 'MSSQL_SA_PASSWORD=aA123456' \ 
     -e 'MSSQL_PID=Developer' \ 
     -e 'MSSQL_COLLATION=Turkish_CI_AS' \ 
     -e 'MSSQL_TCP_PORT=1433' \ 
     -p 14333:1433 \ 
     --name mssqlserver \ 
     --hostname mssqlserver \ 
     --restart unless-stopped \ 
     -v c:\docker\sql\data:/var/opt/mssql/data \ 
     -v c:\docker\sql\log:/var/opt/mssql/log \ 
     -v c:\docker\sql\backup:/var/opt/mssql/backup \ 
     -v c:\docker\sql\secrets:/var/opt/mssql/secrets \ 
     -d mcr.microsoft.com/mssql/server:2019-latest
```

```zsh
# change sa password
docker exec -it mssqlserver /opt/mssql-tools/bin/sqlcmd `
   -S localhost -U sa -P "<YourStrong!Passw0rd>" `
   -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong!Passw0rd>'"

# create a folder in container
docker exec -it mssqlserver mkdir /var/opt/mssql/newfolder

# copy file from docker container to windows host
docker cp f247994ca42e:/var/opt/mssql/data/MyWordsDB.mdf Desktop/MyWordsDB.mdf
docker cp f247994ca42e:/var/opt/mssql/data/MyWordsDB_log.ldf Desktop/MyWordsDB_log.ldf
```

# MariaDB

```zsh
% docker run -d --restart=always -p 3306:3306 --name MARIA_DB -e MARIADB_ROOT_PASSWORD=aA123456 -v maria_data:/var/lib/mysql mariadb:latest
```

```zsh
% docker exec -it MARIA_DB mariadb --user root -p aA123456
```

# PostgreSQL

```zsh
% docker run -d --restart=always --name POSTGRES_DB -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=aA123456 -p 5432:5432 -v postgres_data:/var/lib/postgresql/data postgres:latest -c listen_addresses='*'
```

```zsh
% docker exec -it POSTGRES_DB psql -U postgres
% docker exec -it POSTGRES_DB bash
% cat /var/lib/postgresql/data/postgresql.conf | grep "listen"
```


# PgAdmin

```zsh
% docker run -d --restart=always --name PGADMIN -p 5050:80 -e PGADMIN_DEFAULT_EMAIL=emumcu@outlook.com -e PGADMIN_DEFAULT_PASSWORD=aA123456 -v pgadmin_data:/var/lib/pgadmin dpage/pgadmin4:latest
```

**To connect a postgresql instance running in a docker container use:**

`host.docker.internal` as host name.

# Postgres Sample Database
https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/ 

## How to Restore Data Dump Using pg_restore

### Step 1: Find the name and id of the Docker container hosting the Postgres instance

```zsh
% docker ps
```

CONTAINER ID		NAMES
3213a9d64a61   		POSTGRES_DB

### Step 2: Find the volumes available in the Docker container

```zsh
% docker inspect -f '{{ .Mounts }}' containerid
```

Look at the volume paths under the key Destination.

```zsh
% docker inspect -f '{{ .Mounts }}' 3213a9d64a61
```

```
[{volume postgres_data /var/lib/docker/volumes/postgres_data/_data  /var/lib/postgresql/data local z true }]
docker inspect 3213a9d64a61
        "Mounts": [
            {
                "Type": "volume",
                "Name": "postgres_data",
                "Source": "/var/lib/docker/volumes/postgres_data/_data",
                "Destination": "/var/lib/postgresql/data",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
```

### Step 3: Copy dump into one of the volumes

```zsh
% docker cp </path/to/dump/in/host> <container_name>:<path_to_volume>
% docker cp /Users/emre/Downloads/dvdrental.tar POSTGRES_DB:/var/lib/postgresql/data
```

### Step4: Create database

```zsh
% docker exec -it POSTGRES_DB bash
% psql -Upostgres
PSQL> create database dvdrental;
PSQL> \q
```

### Step5: Restore Database

```zsh
% docker exec -it POSTGRES_DB bash
% pg_restore -U postgres -d dvdrental /var/lib/postgresql/data/dvdrental.tar
```

# Docker Commands

```zsh
# copy file from windows host to docker container
docker cp file.ext mssqlserver:/var/opt/mssql/newfolder
docker cp .\Desktop\MyWordsDB.mdf ea2c9599dfda:/var/opt/mssql/data
docker cp .\Desktop\MyWordsDB_log.ldf ea2c9599dfda:/var/opt/mssql/data

# Get container IP address
docker inspect CONTAINER_ID | grep IPAddress

# Update containers as restart always
docker update --restart=always ContainerID


docker ps -a
docker inspect mssqlserver

# Some Docker Command Parameters
-v: Volume parameter
-p: [HostPort:ContainerPort]
-e: [environment variables]

```