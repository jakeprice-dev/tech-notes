---
id: 20230111084941
uuid: 7ec82f91-6c72-4eea-90ad-34fb48578438
title: Hash a Password using bcrypt
date: 2023-01-11 08:49:41
modified: 
types: tech-note
categories: cli
link: https://unix.stackexchange.com/a/419855/331627
pinned: false
tags: [bcrypt, hash, password, caddy-security]
private: false
draft: false
---

It's surprisingly not that straightforward to find a tool to create a bcrypt hashed password.

```sh
htpasswd -bnBC 10 "" <password> | tr -d ':\n' | sed 's/$2y/$2a/'
```

More on [StackExchange](https://unix.stackexchange.com/a/419855/331627)
