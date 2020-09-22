---
id: 20200922125020
uuid: 820571f3-bca2-40c5-a2e1-db682ec2e5ab
title: Check Size of a Table in PostgreSQL
date: 2020-09-22 12:50:20
modified: 
types: tech-note
categories: postgresql
pinned: false
tags: [postgresql, psql, tables, size, database]
private: false
draft: false
---

To get an overview of how much space is taken by each table in a database.

```sh
psql --host=<host> --dbname=<dbname> --username=<username> --command='SELECT relname as "Table", pg_size_pretty(pg_total_relation_size(relid)) As "Size", pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) as "External Size" FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC;'
```

The SQL above, in a prettier format looks like this:

```sql
SELECT
   relname as "Table",
   pg_size_pretty(pg_total_relation_size(relid)) As "Size",
   pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) as "External Size"
   FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC;
```

Get the size of all objects:

```sh
psql --host=<host> --dbname=<dbname> --username=<username> --command='SELECT relname AS objectname, relkind AS objecttype, reltuples AS "#entries", pg_size_pretty(relpages::bigint*8*1024) AS size FROM pg_class WHERE relpages >= 8 ORDER BY relpages DESC;' > db_table_object_size.txt
```

Again, the SQL, in a prettier format:

```sql
SELECT
   relname AS objectname,
   relkind AS objecttype,
   reltuples AS "#entries", pg_size_pretty(relpages::bigint*8*1024) AS size
   FROM pg_class
   WHERE relpages >= 8
   ORDER BY relpages DESC;
```
