---
id: 20200329220701
uuid: 31f7f32d-e368-46ed-b481-bde887db8c13
title: List Apps with ADB
date: 2020-03-29 22:07:01
modified: 
types: tech-note
categories: android
pinned: false
tags: [adb, android, apps, search, find, filter]
private: false
draft: false
---

```sh
# List Android apps and the app name:
adb shell pm list packages -f -s | sort

# Search for and list:
adb shell "pm list packages -f tplink"
```


