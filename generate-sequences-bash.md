---
id: 20200623143706
uuid: 4c7c1951-f8f5-46c9-abbc-1d6f51667207
title: Generate Sequences in Bash
date: 2020-06-23 14:37:06
modified: 
types: tech-note
categories: bash
pinned: false
tags: [bash, loop, iterate]
private: false
draft: false
---

You can generate sequences in Bash using the following syntax.

```sh
# Syntax:
for i in {1..200}; do <command>; done

# Example:
for i in {1..200}; do printf "id: %s\n" "$(uuidgen)"; done
```
