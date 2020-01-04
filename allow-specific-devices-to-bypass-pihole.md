---
id: 20200104221244
uuid: ecd2f29e-528f-43cb-8037-a1dce9fc5490
title: Allow Specific Devices to Bypass Pi-hole or dnsmasq
date: 2020-01-04 22:12:44
modified: 
types: tech-note
categories: pihole
pinned: false
tags: [mac-address, network, pihole, dnsmasq, override, bypass]
private: false
draft: false
---

If you want a specific device to bypass Pi-hole and use another DNS server you can do it by adding an exception to `/etc/dnsmasq.d/04-bypass.conf`.

Append something like this to the bottom of the file.

This of course also works with dnsmasq on it's own, you can add something like the below directly to the `dnsmasq.conf` file.

```ini
## This will go straight to Googles DNS Servers.
dhcp-option=tag:googlesdns1,6,8.8.8.8
dhcp-option=tag:googlesdns2,6,8.8.4.4

## Your Device that goes to Google DNS
# dhcp-host=MA:CA:DD:R:ES:SS,set:googlesdns1
dhcp-host=10:8E:E0:88:E3:F0,set:googlesdns1 # phone
dhcp-host=F0:1D:BC:07:27:02,set:googlesdns1 # pc
```

A full example below from [here](https://github.com/deathbybandaid/piadvanced/blob/master/piholetweaks/dnsmasqtweaks/04-bypass.conf):

```ini
## The contents here bypass pihole by mac address

## This will go straight to Googles DNS Servers.
dhcp-option=tag:googlesdns1,6,8.8.8.8
dhcp-option=tag:googlesdns2,6,8.8.4.4

## This will go straight to Opendns Servers.
dhcp-option=tag:opendns1,6,208.67.222.220
dhcp-option=tag:opendns2,6,208.67.222.222
## OpenDNS FamilyShield
dhcp-option=tag:opendns3,6,208.67.222.123
dhcp-option=tag:opendns4,6,208.67.220.123

## Level3 DNS
dhcp-option=tag:Level3DNS1,6,4.2.2.1
dhcp-option=tag:Level3DNS2,6,4.2.2.2

## Comodo Secure DNS
dhcp-option=tag:ComodoSecureDNS,6,8.26.56.26
dhcp-option=tag:ComodoSecureDNS,6,8.20.247.20

## Norton
## P1 - malware, phishing schemes, and scams.
dhcp-option=tag:norton1,6,199.85.126.10
dhcp-option=tag:norton2,6,199.85.127.10
## P2 - Pornography
dhcp-option=tag:norton3,6,199.85.126.20
dhcp-option=tag:norton4,6,199.85.127.20
## P3 - Security + Pornography + Other
dhcp-option=tag:norton5,6,199.85.126.30
dhcp-option=tag:norton6,6,199.85.127.30

########################################################################

## Instructions
## First set your tag, and dns server.
## dhcp-option=tag:YOURTAGHERE,6,IPADDRESSOFDNSSERVER
## You then simply need to replace MA:CA:DD:R:ES:SS
## I have set up the standard DNS servertags above.
## Below are examples of how to set a mac address to bypass pihole.

## Your Device that goes to Google DNS
#dhcp-host=MA:CA:DD:R:ES:SS,set:googlesdns1

## Your Device that goes to OpenDNS
#dhcp-host=MA:CA:DD:R:ES:SS,set:opendns1

## Your Device that goes to custom DNS Server
#dhcp-host=MA:CA:DD:R:ES:SS,set:YOURTAGHERE
```
