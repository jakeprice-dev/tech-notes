---
id: 20200620232643
uuid: 1f677e56-3332-4990-a5d4-9c32caf4849e
title: rsync Cheatsheet
date: 2020-06-20 23:26:43
modified: 
types: tech-note
categories: cli
pinned: false
tags: [rsync, backups]
private: false
draft: false
---

```sh
# Backup `my` directory to `jp-archive-1` external drive.
rsync --verbose --recursive --perms --xattrs --times --human-readable $HOME/my /run/media/jprice/jp-archive-1/backups/
```
