---
id: 20220119205159
uuid: 3bf4ba87-4adf-4afb-8988-d82d6a070f89
title: Find out Laptop Screen Panel Model
date: 2022-01-19 20:51:59
modified: 
types: tech-note
categories: linux
link: 
pinned: false
tags: [laptop, lcd, panel, edid, edp]
private: false
draft: false
---

```sh
sudo dnf install edid-decode
cat /sys/class/drm/card0-eDP-1/edid | edid-decode
```

