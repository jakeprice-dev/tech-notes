---
id: 20210902150532
uuid: 6fe0984f-610d-4211-80c2-c758441f7e32
title: Create a QR Code
date: 2021-09-02 15:05:32
modified: cli
types: tech-note
categories: 
pinned: false
tags: [qrencode, wifi, credentials, password]
private: false
draft: false
---

## Wi-Fi

Some smartphones have the ability to connect to Wi-Fi by scanning a QR code. You can use the `qrencode` tool to do this.

Here's the standard template:

```text
WIFI:S:<ssid>;T:<security-type>;P:<password>;;
```

Here's a couple of examples using `qrencode`, which you'll need to install.

```sh
# Example (print the output on the terminal):
qrencode --type=ansiutf8 "WIFI:S:<ssid>;T:WPA;P:<password>;;"

# Example (print the output to a file, which you can then print):
qrencode \
    --type=PNG \
    --size=6 \
    --foreground="4caf50" \
    --background="FFFFFF" \
    --output ppn5.png \
    "WIFI:S:PPN5;T:WPA;P:Groggily*Exclude1*Sublevel*Denial;;"
```

## Misc Information

```bash
# Multiline:
qrencode \
    --type=PNG \
    --size=6 \
    --foreground="006a4d" \
    --background="FFFFFF" \
    --output output.png \
    """
Multiline
QR Code
With Text
    """
```
