---
id: 20221025151042
uuid: 2895f11b-c9ff-412b-b223-18c775ed515a
title: Wrap systemctl output
date: 2022-10-25 15:10:42
modified: 
types: tech-note
categories: cli
link: 
pinned: false
tags: [systemctl, systemd, wrap, log]
private: false
draft: false
---

If you're checking the logs of a systemd service, and they are going off the screen, although you can use the arrow keys to view the rest of the line, you can also just stop it from happening with the below flags.

```sh
$ sudo systemctl status <service> --full --no-pager
```
