---
id: 20220109174101
uuid: f1f272f1-c910-4662-90bf-a5b8c7b117d7
title: Fix "... Damaged Can't be Opened" on MacOS
date: 2022-01-09 17:41:01
modified: 
types: tech-note
categories: macos
link: https://old.reddit.com/r/MacOS/comments/q9d772/homebrew_chromium_is_damaged_and_cant_be_openend/
pinned: false
tags: [macos, chromium, damaged, xattr, attribute, application]
private: false
draft: false
---

How to fix "Chromium is Damaged Can't Be Opened".

```sh
xattr -cr /Applications/Chromium.app
```
