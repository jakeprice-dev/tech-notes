---
id: 20200508112448
uuid: 9bf4766e-2481-45d2-bd37-5c9e61e5d05c
title: jq Cheatsheet
date: 2020-05-08 11:24:48
modified: 
types: tech-note
categories: cli
tags: [jq, json]
private: false
draft: false
---

```sh
# Take value and add to a new key name.
.[ ] | .[ ] | { Name: .StackName, Status: .StackStatus, Created: .CreationTime }

# Put values into new object:
jq '.[]|{name,id}'
```
