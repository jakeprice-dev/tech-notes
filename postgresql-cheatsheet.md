---
id: 20200804205323
uuid: eab4a299-6e9b-4669-9da3-5a9bde803893
title: PostgreSQL Commands
date: 2020-08-04 20:53:23
modified: 
types: tech-note
categories: postgresql
pinned: false
tags: [sql, commands, statements, psql, postgresql]
private: false
draft: false
---

## Connection Strings

```sh
psql postgresql://<user>@<database_url>:<port>/<database_name>
```

## Run SQL Script

```sh
psql postgresql://<user>@<database_url>:<port>/<database_name> --file=<script>.sql
```

## Extract Data

When using PostgreSQL, you can run statements with the `copy` command below, and extract them into a CSV file.

```sh
psql postgresql://<user>@<database_url>:<port>/<database_name> --command="copy (SELECT * FROM table_id) TO STDOUT WITH CSV DELIMITER ',' HEADER;" > output.csv
```
## Create User/Role

```sql
CREATE USER <username> WITH PASSWORD '<password>';
```

## Statements

```sql
-- Count lines in a table:
SELECT COUNT(*) FROM table_id; 

-- Retrieve all lines from a table:
SELECT * from table_id;

-- Retrieve 10 lines from a table:
SELECT * from table_id limit 10;

-- Select named columns from a table:
SELECT system_user, system_user_login, account_locked FROM system_user;

-- Create an index on a table column:
CREATE INDEX index_name ON table_id (column_id);

-- Get the size of a database:
select pg_size_pretty( pg_database_size('databasename') );

-- Get the size of a table:
select pg_size_pretty( pg_total_relation_size('tablename') );

-- Reset user password:
ALTER USER username PASSWORD 'password';

-- Sort tables in a database, by size:
SELECT
   relname as "Table",
   pg_size_pretty(pg_total_relation_size(relid)) As "Size",
   pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) as "External Size"
   FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC;
```
