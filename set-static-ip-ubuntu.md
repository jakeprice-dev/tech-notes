---
id: 20200628130240
uuid: 2ca351d9-d30d-40f7-baf7-9aefccd9aa18
title: Set Static IP on Ubuntu 20.04
date: 2020-06-28 13:02:40
modified: 
types: tech-note
categories: network
pinned: false
tags: [static, networking, ubuntu]
private: false
draft: false
---

Create or edit `/etc/netplan/01-netcfg.yaml` and insert the below, replacing the adapter name and IP info if required:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      addresses: [10.0.0.2/24]
      gateway4: 10.0.0.1
      nameservers:
        addresses: [1.1.1.1,1.0.0.1]
```

The example above sets a static IP on my Raspberry Pi.

To restart networking, you can then run `sudo netplan apply`.
