---
id: 20200530133452
uuid: e5654663-ea38-4104-80b3-c97f619a56aa
title: Use Dig to Get Public IP
date: 2020-05-30 13:34:52
modified: 
types: tech-note
categories: cli
link: https://unix.stackexchange.com/questions/22615/how-can-i-get-my-external-ip-address-in-a-shell-script)
pinned: false
tags: [dig, linux, ip, dnsutils]
private: false
draft: false
---

Source: [How can I get my external IP address in a shell script?](https://unix.stackexchange.com/questions/22615/how-can-i-get-my-external-ip-address-in-a-shell-script)

A safer and more reliable way than using `curl` to get your public IP is to use `dig` as below:

```sh
# OpenDNS:
dig @resolver1.opendns.com ANY myip.opendns.com +short

# Google:
dig @ns1.google.com TXT o-o.myaddr.l.google.com +short

# Akamai:
dig @ns1-1.akamaitech.net ANY whoami.akamai.net +short
```

Just make sure `dnsutils` is installed if `dig` isn't installed.
