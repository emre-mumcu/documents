# OracleXE in Docker

```zsh
% docker run -d --name oracle-xe -p 1521:1521 -e ORACLE_PWD=aA123456 container-registry.oracle.com/database/express:latest
```

``` zsh
# Connection Parameter
# host      : localhost
# port      : 1521
# username  : system/sys
# password  : aA123456
# sid       : xe
```

If you need to exposure express database administration, add the following port:

``` zsh
-p <host-port>:5500 
-e ORACLE_CHARACTERSET=<your character set>
-v <host mount point>:/opt/oracle/oradata
```

* `/opt/oracle/oradata` is the data volume for the database and required database configuration files.
* `/opt/oracle/scripts/setup` is the directory containing either shell or SQL scripts that are executed once after the database setup (creation) has been completed.
* `/opt/oracle/scripts/startup` is the directory containing either shell or SQL scripts that are executed every time the container starts.

```
-v /host/docker/xe/oradata:/opt/oracle/oradata
-v /host/docker/xe/scripts/setup:/opt/oracle/scripts/setup
-v /host/docker/xe/scripts/startup:/opt/oracle/scripts/startup
```

The data volume `/opt/oracle/oradata` lets you preserve the database’s data and configuration files on the host file system in case the container is deleted. The directory must be writable by a user with UID `54321`, which is the oracle user within the container. There are two ways to ensure this:

Change the ownership of the directory for oracle user

```zsh
% chown 54321:54321 /host/docker/xe/oradata;
```

Or make the directory writable by everyone

```zsh
$ chmod 777 /host/docker/xe/oradata
```

```zsh
% docker exec -it --user=oracle oracle-xe bash
```

# CDB / PDB

In Oracle 12c, a new database architecture or design known as "container databases" or "multitenant architecture" was introduced. The Oracle database can be used as a "multitenant container database," or CDB. This CDB may include one or more "pluggable databases," or PDBs. A PDB is a collection of schemas and objects that seem to applications and IDEs as a "normal" database. 

If you've been working with Oracle for a long and are unfamiliar with the CDB and PDB structures, the easy answer is that a PDB is similar to a "normal database" that you deal with.

## Connecting to a Container Database (CDB)

Connecting to the root of a container database is the same as that of any previous database instance. 

## Connecting to a Pluggable Database (PDB)

Direct connections to pluggable databases must be made using a service. Each pluggable database automatically registers a service with the listener. This is how any application will connect to a pluggable database, as well as administrative connections.

You can query the CDB column in the V$DATABASE view to determine whether a database is a CDB or a non-CDB. The CDB column returns YES if the current database is a CDB or NO if the current database is a non-CDB.

```sql
-- Check if database is running in container mode
-- If the CDB value is YES then the database is running in container mode
SELECT T.CDB, T.* FROM V$DATABASE T;
```

The V$CONTAINERS view provides information about all containers in a CDB, including the root and all PDBs.

```sql
SELECT * FROM V$CONTAINERS
```

The CDB_PDBS view and DBA_PDBS view provide information about the PDBs associated with a CDB, including the status of each PDB.

```sql
SELECT * FROM CDB_PDBS
SELECT * FROM DBA_PDBS
```

Get the name of pluggable databases:

```sql
-- Get the name of pluggable database
SELECT NAME, OPEN_MODE FROM V$PDBS WHERE CON_ID > 2;
```

To switch the selected PDB and create a user:

```sql
ALTER SESSION SET CONTAINER = XEPDB1;
CREATE USER test IDENTIFIED BY aA123456 CONTAINER=CURRENT;
GRANT CREATE SESSION TO test CONTAINER=CURRENT;
-- or
GRANT CREATE SESSION TO test CONTAINER=ALL;
```

To connect the PDB using the credentials created above change the service name to the PDB name.

```
host            : localhost
port            : 1521
username        : test
password        : aA123456
service name    : XEPDB1
```

# If you want to create user in CDB:

```sql
create user c##test identified by aA123456;
```

# If you want to create user in PDB:

```sql
SELECT NAME, OPEN_MODE FROM V$PDBS WHERE CON_ID > 2;
ALTER SESSION SET CONTAINER = XEPDB1;
CREATE USER test IDENTIFIED BY aA123456 CONTAINER=CURRENT;
GRANT CREATE SESSION TO test CONTAINER=CURRENT;
-- or
GRANT CREATE SESSION TO test CONTAINER=ALL;
```