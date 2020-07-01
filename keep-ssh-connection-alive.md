---
id: 20200701212641
uuid: 114c1d46-2549-4ee5-b6f5-9e27549d9bc1
title: Keep an SSH Connection Alive
date: 2020-07-01 21:26:41
modified: 
types: tech-note
categories: cli
pinned: false
tags: [ssh, connection, linux]
private: false
draft: false
---

The option `-o ServerAliveInterval=60` tells the SSH agent to keep sending "pings" to keep the connection alive.

Example below.

```sh
ssh -v -o ServerAliveInterval=60 myname@myhost.com
```

Alternatively you can add it the your ssh config file for all hosts:

```sh
Host *
    ServerAliveInterval 60
```
