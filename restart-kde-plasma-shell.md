---
id: 20210703093745
uuid: b0998816-3192-43cb-9bd2-155a61f5170a
title: Restart KDE Plasma
date: 2021-07-03 09:37:45
modified: 
types: tech-note
categories: cli
pinned: false
tags: [kde, plasma]
private: false
draft: false
---

```sh
# For KDE > 5.10
kquitapp5 plasmashell || killall plasmashell && kstart5 plasmashell
```

> Remarks:
> 1. If you are not sure which KDE version your run, kinfocenter --version will tell you.
> 2. you can skip the `kquitapp5 plasmashell ||` part if you don't want to be stuck in the timeout when plasmashell is not responding.

Original Source: [kwin - Can I restart the KDE Plasma Desktop without logging out? - Ask Ubuntu](https://askubuntu.com/questions/481329/can-i-restart-the-kde-plasma-desktop-without-logging-out)