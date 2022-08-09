---
id: 20220809163152
uuid: ec28bb00-d300-4b1e-af36-8c38cd4b6798
title: journald Cheatsheet
date: 2022-08-09 16:31:52
modified: 
types: tech-note
categories: cli
link: 
pinned: false
tags: [journald, log, follow]
private: false
draft: false
---

```sh
# Follow all logs:
$ sudo journalctl --follow

# Follow logs for a service:
$ sudo journalctl --follow --unit tailscaled

# View logs since date/time:
$ journalctl --since "2022-10-14"
$ journalctl --since "2022-10-14 21:55"
```

