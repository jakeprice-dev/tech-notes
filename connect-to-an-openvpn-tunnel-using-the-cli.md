---
id: 20190914213452
uuid: f897a427-3bf4-4f96-ad5e-89f5efc2b7e0
title: Connect to an OpenVPN Tunnel Using the CLI
date: 2019-09-14 21:34:52
modified: 
types: tech-note
categories: openvpn
pinned: false
tags: [openvpn, ubuntu]
private: false
draft: false
---

Use an OpenVPN configuration file to connect to an OpenVPN tunnel.

``` sh
$ sudo openvpn --config <path-to-openvpn-config-file>
```

The `sudo` is important, otherwise it may fail to connect. 
