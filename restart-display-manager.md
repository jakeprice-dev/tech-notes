---
id: 20221014192344
uuid: b043e38d-2273-448f-9dd0-270b6aeaccd7
title: Restart Display Manager
date: 2022-10-14 19:23:44
modified: 
types: tech-note
categories: cli
link: 
pinned: false
tags: [fedora, linux, monitor, multiple-monitor, screen, displayport, usb-c, dock]
private: false
draft: false
---

When I connect my EliteBook to my Dock and Daisy Chain USB-C cable, sometimes there are issues with the screens coming on.

Historically I've just tended to restart, but you don't need to, you can just restart the display manager, as below.

```sh
$ sudo systemctl restart display-manager
```

This will log you out, so it's almost as inconvenient as a restart, but it tends to fix any issues I'm having.

