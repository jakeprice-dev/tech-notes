---
id: 20210209123311
uuid: 39f73f6f-11e1-4a07-bc21-b118260c5885
title: SQLite3 Cheat Sheet
date: 2021-02-09 12:33:11
modified: 
types: tech-note
categories: sqlite
pinned: false
tags: [cheatsheet, sql, sqlite, database]
private: false
draft: false
---

```sh
# Run script on database:
sqlite3 example.db
sqlite> .read create.sql

# Run script on database:
sqlite3 example.db < create.sql

# Run query on database:
sqlite3 example.db 'CREATE TABLE entries(id TEXT, title TEXT, date TEXT, tags TEXT, content TEXT);'

# Delete row where column equals:
sqlite3 wiki.db "DELETE FROM entries WHERE id=20210209145248;"

# Delete row:
DELETE FROM "main"."entries" WHERE _rowid_ IN ('918');
```
