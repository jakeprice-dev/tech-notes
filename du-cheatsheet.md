---
id: 20200705194016
uuid: 2cdf7e39-e5ab-4517-bbae-0b1846269fe4
title: File Space Usage with du
date: 2020-07-05 19:40:16
modified: 
types: tech-note
categories: cli
pinned: false
tags: [disk-usage, du, bash, linux]
private: false
draft: false
---

```sh
# Show disk usage.
du --human-readable --max-depth=1

# Show and then sort disk usage.
du --human-readable --max-depth=1 | sort -n
```
