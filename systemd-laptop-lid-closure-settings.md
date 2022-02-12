---
id: 20220212164324
uuid: 6293d7b0-f206-49ad-8b40-ed52c6a3493f
title: Laptop Lid Closure Settings on Linux
date: 2022-02-12 16:43:24
modified: 
types: tech-note
categories: systemd
link: https://unix.stackexchange.com/a/583059/331627
pinned: false
tags: [screen, laptop, lid, sleep, suspend, monitor, power]
private: false
draft: false
---

```sh
# Don't go to sleep if lid closed and monitor connected:
sed --in-place .backup 's/#HandleLidSwitchDocked=ignore/HandleLidSwitchDocked=ignore/' /etc/systemd/logind.conf

$ /etc/systemd/logind.conf
HandleLidSwitch=suspend                # suspend when on battery
HandleLidSwitchExternalPower=ignore    # inhibit suspend when on AC 
HandleLidSwitchDocked=ignore           # inhibit suspend when on external monitor

$ reboot
# ↓ or ↑
$ sudo systemctl restart systemd-logind  # will kill current X/Wayland session
```

