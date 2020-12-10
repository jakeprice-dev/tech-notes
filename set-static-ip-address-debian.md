---
id: 20201210101715
uuid: ed99730f-0dbf-40a2-ab79-1431bd5bd8e2
title: Set Static IP Address on Debian
date: 2020-12-10 10:17:15
modified: 
types: tech-note
categories: network
pinned: false
tags: [debian, networking, ip, linux]
private: false
draft: false
---

{{<admonition important>}}
There's a not insignificant chance you'll get kicked out of the server, so you should be physically present to reboot it to get back in. Don't do it on a server you can't access.
{{</admonition>}}

```sh
# File: /etc/network/interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug eno1
#iface eno1 inet dhcp
```

```sh
# File: /etc/network/interfaces.d/eno1

iface eno1 inet static
        address 10.0.0.226
        netmask 255.255.255.255
        gateway 10.0.0.1
```
