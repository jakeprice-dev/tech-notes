---
id: 20190305094409
uuid: ec9eff1d-18e0-4560-a0c3-418a77915f73
title: Retrieve Nameservers for Hostname
date: 2019-03-05 09:44:09
modified: 
types: tech-note
categories: cli
pinned: false
tags: [nameserver, dns, hostname, domain]
private: false
draft: false
---

Return the nameservers for a hostname.

```sh
$ host -t ns domain.com
```

For other records replace `ns` with one of the following.

```sh
a       ip address
mx      mail servers
ns      name servers
txt     txt record
cname   cname record
soa     soa record
any     all details
```
