---
id: 20230123162835
uuid: 254b9cbf-4283-463f-a1e4-ae5a11374c74
title: Fix for Docker Breaking KVM Networking
date: 2023-01-23 16:28:35
modified: 
types: tech-note
categories: network
link: https://wiki.archlinux.org/title/Docker#Starting_Docker_breaks_KVM_bridged_networking
pinned: false
tags: [docker, kvm, bridge, network]
private: false
draft: false
---

Detailed on the ArchWiki below is a solution to an issue I was having when I tried to use a network bridge `br0` _alongside_ Docker on my server.

The solution that I thought worked initially was to add the below to `/etc/docker/daemon.json`. This got the bridge working for virtual machines, but broke Docker container external connectivity (containers couldn't access the internet).

```json
{
  "bridge": "br0"
}
```

In the end I needed to run the `iptables` rule _instead_, and that got things working for both Docker and the Virtual Machines.

```sh
iptables -I FORWARD -i br0 -o br0 -j ACCEPT
```
