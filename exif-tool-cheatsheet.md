---
id: 20211229140507
uuid: d1e41021-e0b0-41e9-adb5-b3d29bae8661
title: Exif Tool Cheatsheat
date: 2021-12-29 14:05:07
modified: 
types: tech-note
categories: cli
link: 
pinned: false
tags: [exif, photo, video, tag]
private: false
draft: false
---

More info [here](https://exiftool.org/filename.html)

```sh
# Dry-run in current directory:
exiftool -d %Y%m%d_%H%M%S-c.%%e "-testname<CreateDate" .

# Rename all files with "CreateDate" in directory and add copy number if required:
exiftool -d %Y%m%d_%H%M%S%%-c.%%e "-filename<CreateDate" .
```
