---
id: 20200908190455
uuid: 33bbda91-a92f-4922-9641-1f3cea8ee82b
title: Connect to Fedora from Windows with xRDP
date: 2020-09-08 19:04:55
modified: 
types: tech-note
categories: fedora
pinned: false
tags: [fedora, linux, xrdp, windows]
private: false
draft: false
---

It's very hard to get a straight, consistent answer on this, and, I only found one comment on an obscure site that finally helped me fix a constant black screen every time I connected. There is so much rubbish advice on xRDP out there!

The below instructions work on Fedora 33. If you are logged into the desktop, you will just get a black screen when you try to RDP. So you must log out.

```sh
sudo dnf install --assumeyes xrdp xorgxrdp tigervnc-server 
sudo systemctl enable --now xrdp
sudo firewall-cmd --add-port=3389/tcp --permanent
sudo firewall-cmd --reload 
```

Get the machine's IP address `ip addr` and connect to it with RDP from Windows.

If you get multiple prompts to authorise when opening Firefox, [uninstall](https://bugzilla.redhat.com/show_bug.cgi?id=1478345) `pcsc` by running `dnf remove -y pcsc*`.
