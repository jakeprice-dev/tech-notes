---
id: 20210726143738
uuid: 1a44a840-a119-4997-b5ba-b09386d9d88e
title: Recursively Change Directory and File Permissions
date: 2021-07-26 14:37:38
modified: 
types: tech-note
categories: cli
pinned: false
tags: [find, chmod, recursive, permission]
private: false
draft: false
draft: false
---

If you've messed up the permissions in a directory, you can recursively fix it with `find`.

```sh
# Operate on directories:
find <path> -type d -exec chmod 755 {} \;

# Operate on files:
find <path> -type f -exec chmod 644 {} \;
```
