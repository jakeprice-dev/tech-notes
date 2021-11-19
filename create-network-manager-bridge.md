---
id: 20211119213636
uuid: d36a2fba-c59d-4b80-9077-e790f6376488
title: Creating a Bridge with NetworkManager
date: 2021-11-19 21:36:36
modified: network
types: tech-note
categories: network
link: https://wiki.archlinux.org/title/Network_bridge#With_NetworkManager
pinned: false
tags: [arch-wiki, nmcli, network-manager, bridge, ethernet, interface, br0]
private: false
draft: false
---

Source: [Network bridge - ArchWiki](https://wiki.archlinux.org/title/Network_bridge#With_NetworkManager)
Local: [Here](attachments/20211119213636_Network_bridge.html)

_Important bits excerpted below_.

---

## Creating a Bridge

### With NetworkManager

[GNOME](https://wiki.archlinux.org/title/GNOME "GNOME")'s Network settings can create bridges, but currently will not auto-connect to them or slave/attached interfaces. Open Network Settings, add a new interface of type Bridge, add a new bridged connection, and select the MAC address of the device to attach to the bridge.

[KDE](https://wiki.archlinux.org/title/KDE "KDE")'s [plasma-nm](https://archlinux.org/packages/?name=plasma-nm) can create bridges. In order to view, create and modify bridge interfaces open the Connections window either by right clicking the Networks applet in the system tray and selecting _Configure Network Connections..._ or from _System Settings &gt; Connections_. Click the _Configuration_ button in the lower left corner of the module and enable "Show virtual connections". A session restart will be necessary to use the enabled functionality.

[nm-connection-editor](https://archlinux.org/packages/?name=nm-connection-editor) can create bridges in the same manner as GNOME's Network settings.

`nmcli` from [networkmanager](https://archlinux.org/packages/?name=networkmanager) can create bridges. Creating a bridge with [STP](https://en.wikipedia.org/wiki/Spanning_Tree_Protocol "wikipedia:Spanning Tree Protocol") disabled (to avoid the bridge being advertised on the network):

    $ nmcli connection add type bridge ifname br0 stp no

Making interface `enp30s0` a slave to the bridge:

    $ nmcli connection add type bridge-slave ifname enp30s0 master br0

Setting the existing connection as down (you can get it with `nmcli connection show --active`):

    $ nmcli connection down _Connection_

Setting the new bridge as up:

    $ nmcli connection up bridge-br0

If NetworkManager's default interface for the device you added to the bridge connects automatically, you may want to disable that by clicking the gear next to it in Network Settings, and unchecking "Connect automatically" under "Identity."

## Assigning an IP address

### With NetworkManager

Give it the desired address:

    nmcli connection modify _interface_ ipv4.addresses _desired_IP_

Set up a DNS server (this will also avoid not being able to load any pages after you apply the changes):

    nmcli connection modify _interface_ ipv4.dns _DNS_server_

Set the IP address to static:

    nmcli connection modify _interface_ ipv4.method manual

Apply the changes:

    nmcli connection up _interface_
