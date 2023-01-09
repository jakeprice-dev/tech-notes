---
id: 20230109141437
uuid: 417a489b-4714-4a49-8b7d-550ee56e20b5
title: WireGuard Troubleshooting
date: 2023-01-09 14:14:37
modified: 
types: tech-note
categories: network
link: 
pinned: false
tags: [vpn, wireguard, troubleshoot]
private: false
draft: false
---

## Port Forward Rule

Make sure the rule's protocol is `UDP`.

## nmap

Check if the port is open.

```sh
sudo nmap -sU -p 51820 <ip-or-endpoint>
```

## tcpdump

Check is traffic is flowing on the interface.

```sh
sudo tcpdump -nn -i wg0
```

## Last Handshake

On the main server, check if the peer has ever, or recently connected, by looking at the `latest handshake`.

```sh
$ sudo wg show
interface: wg0
  public key: <value>
  private key: (hidden)
  listening port: 51820

peer: <value>
  endpoint: <public-ip>:37451
  allowed ips: 10.8.0.2/32
  latest handshake: 7 seconds ago
  transfer: 68.22 KiB received, 267.18 KiB sent
```

