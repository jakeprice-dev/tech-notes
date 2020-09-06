---
id: 20200906152626
uuid: 4ca9649a-351f-46d4-bf24-c7a30ac262a1
title: Access Android Phone Directories On CLI
date: 2020-09-06 15:26:26
modified: 
types: tech-note
categories: android
pinned: false
tags: [android, gvfs, samsung, galaxy-s20, mtp]
private: false
draft: false
---

Using the GUI, especially Nautilus to copy photos off an Android phone can be really slow, this process makes it much more reliable.

Firstly, find out where the device is mounted.

```sh
mount | grep  'gvfsd-fuse'
```

That'll return something like the below:

```sh
gvfsd-fuse on /run/user/1000/gvfs type fuse.gvfsd-fuse (rw,nosuid,nodev,relatime,user_id=1000,group_id=1000)
```

You can then `cd` all the way in to that directory.

```sh
cd /run/user/1000/gvfs/mtp:host=SAMSUNG_SAMSUNG_Android_RF8N30TLG3M/Phone
```
