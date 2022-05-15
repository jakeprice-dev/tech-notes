---
id: 20220515221500
uuid: 72987850-7ff1-48b5-b326-13c99b90ab73
title: Install HP Printer on Fedora
date: 2022-05-15 22:15:00
modified: 
types: tech-note
categories: fedora
link: https://www.cyberciti.biz/faq/how-to-install-networked-hp-printer-and-scanner-on-fedora-linux/
pinned: false
tags: [hp, printer, fedora, hplib, hplip]
private: false
draft: false
---

The below article provided some pointers with some recent HP Printer issues I had on Fedora 36. What got it working was running `hp-setup 192.168.2.250` after making sure I had installed `sudo dnf install hplip hplip-common hplip-libs libsane-hpaio hplip-gui`.

{{<embed_html file="/attachments/20220515221500.html">}}

