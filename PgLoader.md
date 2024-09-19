# pgloader

How to use pgloader:

```zsh
% docker exec -it POSTGRES_DB bash
psql -Upostgres
create database MumcuBlog1; 
exit
exit
```

```zsh
% pgloader mysql://emre:'**pass**'@80.240.19.57:3306/MumcuBlog postgresql://postgres:'aA123456'@127.0.0.1:5432/MumcuBlog

% pgloader mysql://emre:'}+/XLSt|QSotm-(Gu96&U9quLv?u%o'@80.240.19.57:3306/MumcuBlog postgresql://postgres:'aA123456'@127.0.0.1:5432/mumcublog1

#or

vi pg.load
load database
from mysql://emre:'}+/XLSt|QSotm-(Gu96&U9quLv?u%oAV'@80.240.19.57:3306/MumcuBlog
into postgresql://postgres:'aA123456'@127.0.0.1:5432/mumcublog
;

load database
from mysql://emre:'**pass**'@80.240.19.57:3306/MumcuBlog
into postgresql://postgres:'aA123456'@127.0.0.1:5432/mumcublog
alter schema 'mumcublog' rename to 'siyah'
;

% pgloader pg.load
```

PostgreSQL, also known as “Postgres,” is an open-source object relational database management system. pgloader is an open-source database migration tool that load data from a database connection into PostgreSQL. 

pgloader has two modes of operation. It can either load data from files, such as CSV or Fixed-File Format; or migrate a whole database to PostgreSQL. It supports migrations from several different database systems including MSSQLServer, MySQL, MariaDB, and SQLite. In both cases, pgloader uses the PostgreSQL COPY protocol which implements a streaming to send data in a very efficient way.

```zsh
% brew install pgloader
% pgloader SOURCE TARGET --dry-run --no-ssl-cert-verification 
```

# MySQL (MariaDB) to PostgreSQL

pgloader mysql://myuser@myhost/dbname pgsql://pguser@pghost/dbname

pgloader mysql://user:password@host:port/dbname pgsql://user:password@host:port/dbname

Using advanced options and a load command file

```zsh
$ pgloader my.load
```

The contents of the command file my.load:

```
LOAD DATABASE
     FROM      mysql://root@localhost/sakila
     INTO postgresql://localhost:54393/sakila

 WITH include drop, create tables, create indexes, reset sequences,
      workers = 8, concurrency = 1,
      multiple readers per thread, rows per range = 50000

  SET PostgreSQL PARAMETERS
      maintenance_work_mem to '128MB',
      work_mem to '12MB',
      search_path to 'sakila, public, "$user"'

  SET MySQL PARAMETERS
      net_read_timeout  = '120',
      net_write_timeout = '120'

 CAST type bigint when (= precision 20) to bigserial drop typemod,
      type date drop not null drop default using zero-dates-to-null,
      -- type tinyint to boolean using tinyint-to-boolean,
      type year to integer

 MATERIALIZE VIEWS film_list, staff_list

 -- INCLUDING ONLY TABLE NAMES MATCHING ~/film/, 'actor'
 -- EXCLUDING TABLE NAMES MATCHING ~<ory>
 -- DECODING TABLE NAMES MATCHING ~/messed/, ~/encoding/ AS utf8
 -- ALTER TABLE NAMES MATCHING 'film' RENAME TO 'films'
 -- ALTER TABLE NAMES MATCHING ~/_list$/ SET SCHEMA 'mv'

 ALTER TABLE NAMES MATCHING ~/_list$/, 'sales_by_store', ~/sales_by/
  SET SCHEMA 'mv'

 ALTER TABLE NAMES MATCHING 'film' RENAME TO 'films'
 ALTER TABLE NAMES MATCHING ~/./ SET (fillfactor='40')

 ALTER SCHEMA 'sakila' RENAME TO 'pagila'

 BEFORE LOAD DO
   $$ create schema if not exists pagila; $$,
   $$ create schema if not exists mv;     $$,
   $$ alter database sakila set search_path to pagila, mv, public; $$;

```