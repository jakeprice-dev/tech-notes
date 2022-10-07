---
id: 20221007213334
uuid: e7a31ab1-62d6-4f58-8285-2968d4c51bfe
title: Reboot to BIOS/UEFI with systemd
date: 2022-10-07 21:33:34
modified: 
types: tech-note
categories: systemd
link: https://www.freedesktop.org/software/systemd/man/systemctl.html#--firmware-setup
pinned: false
tags: [systemd, systemctl, reboot, restart, bios, uefi, firmware]
private: false
draft: false
---

Reboot to your device's BIOS/UEFI with systemd, by using the `systemctl` command.

```sh
$ systemctl reboot --firmware-setup
```

