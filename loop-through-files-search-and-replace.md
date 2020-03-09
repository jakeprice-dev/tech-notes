---
id: 20200309201320
uuid: 4d3bf7a5-1fa2-4bd0-9639-d1657aa7552d
title: Loop Through Files and Search and Replace
date: 2020-03-09 20:13:20
modified: 
types: tech-note
categories: bash
pinned: false
tags: [bash, sed, loop, iteration]
private: false
draft: false
---

Loop through all files, and run a search and replace with `sed`.

```sh
for i in *.txt; do
  sed -i "s/Date::/date:/g" $i;
done
```
