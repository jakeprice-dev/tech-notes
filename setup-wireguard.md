---
id: 20230108122134
uuid: 7dc1a2a7-07ae-4225-8bd6-07657a1c3709
title: Setup WireGuard on Debian
date: 2023-01-08 12:21:34
modified: 
types: tech-note
categories: network
link: https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-22-04
pinned: false
tags: [wireguard, vpn]
private: false
draft: false
---

{{<toc>}}

I've extracted what I needed to do from the clearest article I found on setting up WireGuard - [How To Set Up WireGuard on Ubuntu 22.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-22-04) 

Local Copy: [Here](/attachments/20230109140024-how-to-set-up-wireguard-on-ubuntu-22-04.html)

{{<admonition important>}}
You must set `UDP` as the protocol when setting up the port forwarding rule on your router. Initially, I missed this and things were not working, with no obvious reason for ages (like 48 hours before I realised)!

![](/attachments/2023-01-09_13-57.png)
{{</admonition>}}

## Server

### Install WireGuard & Extras

```sh
sudo apt update
sudo apt install ufw wireguard wireguard-tools
```

### Generate Public & Private Key

#### Private Key

```sh
wg genkey | sudo tee /etc/wireguard/private.key
sudo chmod go= /etc/wireguard/private.key
```

#### Public Key

```sh
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
```

### Create WireGuard Server Configuration File

Create a WireGuard configuration file at `/etc/wireguard/wg0.conf`.

```ini
[Interface]
PrivateKey = <private-key-goes-here>
Address = 10.8.0.1/24
ListenPort = 51820
SaveConfig = true
```

### Enable IP Forwarding

Uncomment the below line in `/etc/sysctl.conf`.

```sh
net.ipv4.ip_forward=1
```

Reload sysctl.

```sh
sudo sysctl -p
```

### Update WireGuard Server Configuration with Firewall Rules

Add the below rules to your `wg0.conf`, and make sure that `eth0` is your primary network interface's name.

```ini
PostUp = ufw route allow in on wg0 out on eth0
PostUp = iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
PreDown = ufw route delete allow in on wg0 out on eth0
PreDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```

### Open Up Firewall Ports

```sh
sudo ufw allow 51820/udp
sudo ufw allow OpenSSH
sudo ufw disable
sudo ufw enable
```

### Start WireGuard on the Server

The server's `wg0.conf` should now look like the below.

```ini
[Interface]
PrivateKey = <private-key-goes-here>
Address = 10.8.0.1/24
ListenPort = 51820
PostUp = ufw route allow in on wg0 out on eth0
PostUp = iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
PreDown = ufw route delete allow in on wg0 out on eth0
PreDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
SaveConfig = true
```

You can now start it using `systemd`.

```sh
sudo systemctl enable --now wg-quick@wg0.service
```

## Peer

### Install WireGuard

Install WireGuard on the peer.

### Generate Public & Private Key

#### Private Key

```sh
wg genkey | sudo tee /etc/wireguard/private.key
sudo chmod go= /etc/wireguard/private.key
```

#### Public Key

```sh
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
```

### Create WireGuard Server Configuration File

Create a WireGuard configuration file at `/etc/wireguard/wg0.conf`.

```ini
[Interface]
PrivateKey = <peer-private-key>
Address = 10.8.0.2/24
DNS = 10.19.90.5

[Peer]
PublicKey = <servers-public-key>
AllowedIPs = 10.8.0.0/24
Endpoint = <server-ip-or-hostname>:51820
```

### Add Peer's Public Key to WireGuard Server

Copy the contents of the peer's public key.

```sh
sudo cat /etc/wireguard/public.key
```

Then from the WireGuard server, run the following.

```sh
sudo wg set wg0 peer <peers-public-key> allowed-ips 10.8.0.2
```

Restart the WireGuard service if running, and then check the peer has been added, either by `cat /etc/wireguard/wg0.conf` or `wg show`.

### Connect from Peer to Server

On the peer, you can bring the WireGuard tunnel up, by running `wg-quick`.

```sh
wg-quick up wg0
```

To bring the tunnel down, run the below.

```sh
wg-quick down wg0
```

