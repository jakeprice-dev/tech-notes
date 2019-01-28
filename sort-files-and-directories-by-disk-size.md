---
id: 20190128125201
uuid: 2a700633-f389-476a-ba70-f79f4ed8288b
title: Sort files and directories by disk size
date: 2019-01-28 12:52:01
modified: 
types: tech-note
categories: cli
pinned: false
tags: [file, directory, size]
private: false
draft: false
---

Sort files and directories by human-readable disk size order.

```sh
$ du --summarize --human-readable * | sort --human-numeric-sort --reverse
```
