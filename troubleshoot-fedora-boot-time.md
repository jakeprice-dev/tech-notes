---
id: 20220116182509
uuid: 686548c1-ef5a-49c1-bd6d-6a4644ee5215
title: Speed-up Fedora Boot Time
date: 2022-01-16 18:25:09
modified: 
types: tech-note
categories: systemd
link: 
pinned: false
tags: [fedora, boot, systemd, analyze, blame, unbound, network-manager]
private: false
draft: false
---

Run the below commands to see what takes the longest at boot.

```sh
sudo systemd-analyze
sudo systemd-analyze blame
```

This will return results that look like the below.

```text
19.252s unbound-anchor.service
 9.581s NetworkManager-wait-online.service
 7.122s dracut-initqueue.service
 6.950s systemd-cryptsetup@luks\x2d6201f753\x2d2cbe\x2d4c1f\x2da7df\x2d0c672e432f0e.service
 4.371s plymouth-quit-wait.service
 1.081s lvm2-monitor.service
```

You can then disable long running services which aren't needed.

```sh
sudo systemctl disable --now unbound-anchor.timer
sudo systemctl disable --now NetworkManager-wait-online.service
```

