---
id: 20200831204451
uuid: a80328fb-1547-484b-a2bb-27986b384a4b
title: Change DNS on Fedora
date: 2020-08-31 20:44:51
modified: 
types: tech-note
categories: network
pinned: false
tags: [dns, fedora, network-manager, resolv-conf]
private: false
draft: false
---

This has been tested as working on Fedora 32.

First add the below to the `[main]` section in `/etc/NetworkManager/NetworkManager.conf`

```sh
dns=none
```

This stops Network Manager from overriding `/etc/resolv.conf`.

You can then edit `/etc/resolv.conf` and add a line for your DNS server, as below.

```sh
# Use Pi-hole for DNS:
10.0.0.3
```

You can then check it works by issuing a `dig <address>.com` and you'll see the active DNS server in the `SERVER` section.

```sh
; <<>> DiG 9.11.22-RedHat-9.11.22-1.fc32 <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 7708
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             2       IN      A       0.0.0.0

;; Query time: 21 msec
;; SERVER: 10.0.0.3#53(10.0.0.3)
;; WHEN: Mon Aug 31 21:42:12 BST 2020
;; MSG SIZE  rcvd: 44
```

You may need to also run the below.

```sh
sudo systemctl restart NetworkManager
```
