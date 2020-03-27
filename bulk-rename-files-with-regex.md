---
id: 20200327202003
uuid: e61a7627-6142-4f68-8f20-528e38791615
title: Bulk Rename Regex Pattern
date: 2020-03-27 20:20:03
modified: 
types: tech-note
categories: cli
pinned: false
tags: [regex, bash, rename, perl]
private: false
draft: false
---

Make sure you've installed the Perl rename tool: `sudo dnf install prename`.

```sh
prename --verbose 's/~\d{8}-\d{6}//;' *
```
