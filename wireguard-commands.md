---
id: 20230109140935
uuid: 2d629ce3-ae5a-4e5c-aa28-046677126c8b
title: WireGuard Commands
date: 2023-01-09 14:09:35
modified: 
types: tech-note
categories: network
link: 
pinned: false
tags: [wireguard, troubleshoot, vpn]
private: false
draft: false
---

## Bring WireGuard Interface Up or Down

### wg-quick

If you're not using `systemctl`, then on server or client you run the below. The config file name will be `wg0` unless you've created a config file with a different name.

```sh
wg-quick up <config-file-name>
wg-quick down <config-file-name>
```

### SystemD

You can also, start or stop with `systemctl` which is good because it'll then automatically bring the interface up on boot.

```sh
sudo systemctl enable --now wg-quick@wg0.service
sudo systemctl status wg-quick@wg0.service
sudo systemctl start wg-quick@wg0.service
sudo systemctl stop wg-quick@wg0.service
sudo systemctl restart wg-quick@wg0.service
```

