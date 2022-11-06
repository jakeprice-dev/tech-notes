---
id: 20221106101901
uuid: f3fcafc1-90fb-4383-aca3-fd1dd49b8521
title: Remove all Temporary Unison Files
date: 2022-11-06 10:19:01
modified: 
types: tech-note
categories: cli
link: 
pinned: false
tags: [unison, sync, find, temporary]
private: false
draft: false
---

Find and delete all `.unison.tmp` files.

```sh
# Dry run:
find my/ -type f -name *.unison.tmp

# Find and delete:
find my/ -type f -name *.unison.tmp -delete
```

