---
id: 20220109163143
uuid: 93ae3e5c-6303-49e2-b5db-4bc6e484e99b
title: KVM/QEMU Notes
date: 2022-01-09 16:31:43
modified: 
types: tech-note
categories: libvirt
link: 
pinned: false
tags: [kvm, qemu, vm]
private: false
draft: false
---

## KVM/QEMU

- `virsh` = Virtualisation Support
- libvirt = Open Source API, daemon and management tool for managing KVM, QEMU and other platform virtualisation technology.
- `virt-install` = CLI for creating KVM, Xen, or Linux container guests, using libvirt.

### Delete VM (Domain)

```sh
virsh list
# Shutdown or force stop:
virsh shutdown --domain test-vm-01
virsh destroy --domain test-vm-01

# Delete:
virsh undefine --domain test-vm-01 --remove-all-storage --delete-storage-volume-snapshots
```

`/var/lib/libvirt/images/test-vm-01.qcow2`

### Connect to VM

Use TigerVNC, `brew install --cask tigervnc-viewer` and input the IP and port: 10.19.90.20:5095. It defaults to localhost:5900 or above. You can define with the `--graphics vnc,listen=0.0.0.0,port=5905` flag that `virt-install` offers. Like the below.

### Create and spin up a VM

```sh
virt-install \
	--name test-vm-01 \
    --connect qemu:///system \
    --graphics vnc,listen=0.0.0.0,port=5905 \
    --network bridge=br0 \
	--memory 2096 \
    --disk size=10 \
    --os-variant ubuntu20.04 \
    --cdrom ubuntu-20.04.3-live-server-amd64.iso

# --filesystem allows you to mount a host directory into the guest - so potentially ideal for samba?
```

### Setup network bridge

To make sure VM's are on the LAN, you need to create a bridge.

```sh
# Delete the default KVM created bridge networking interfaces (virbr0 and virb0-nic):
virsh net-destroy default
virsh net-undefine default
```

On Ubuntu, make sure you add the below bridge section to `/etc/netplan/00-installer-config.yaml`

```yaml
network:
  version: 2
  ethernets:
    eno1:
      dhcp4: true
      optional: true
  bridges:
    br0:
      interfaces: [eno1]
      dhcp4: true
      dhcp6: false
```

Then run `sudo netplan apply`, and reboot for good measure. `optional: true` stops issues on boot with potential long wait times.

To make sure a VM get's a static IP, you can give it a specific MAC address, with QEMU, it must start with 52:54:00. 

Make sure you configure it with the `--network` flag as below:

```sh
    --network bridge=br0,mac=52:54:00:29:da:98 \
```

## Volumes

```sh
# List volumes:
virsh vol-list --pool default

# Delete volumes:
virsh vol-delete --pool default --vol host-01-docker-01.qcow2
```

## Pools

```sh
# The directory can be on a different drive, and thus allows you to have certain vm's on certain disks with attached pools:
virsh pool-define-as --name ssd-02-vm-data --type dir --target /srv/data-ssd-02/vm-data/
virsh pool-list --all
virsh pool-build --pool ssd-02-vm-data
virsh pool-start --pool ssd-02-vm-data
virsh pool-autostart --pool ssd-02-vm-data

# virsh pool-destroy ppn-admin && virsh pool-undefine ppn-admin
```

## Sysprep

```sh
# Shutdown running vm:
virsh shutdown --domain <name>

# virt-sysprep edits disks in place, so snapshot the domain you want to sysprep. This creates an internal snapshot, which is stored as part of the qcow2 image.
virsh snapshot-create-as --domain host-01-docker-01 --atomic ubuntu-2004-base

# Query a domain image (will list snapshots):
sudo su - -c "qemu-img info /srv/data-ssd-02/vm-data/host-01-docker-01.qcow2"
```

---

## Fedora QEMU

### Install

Install packages.

```sh
sudo dnf install qemu-kvm libvirt libguestfs-tools virt-install virt-viewer
```

Add user to `libvirt` group to avoid credentials prompt when launching a VM.

```sh
usermod -aG libvirt $USER
```

### Create Full Network Bridge

Delete default bridge. 

```sh
sudo virsh net-destroy default
sudo virsh net-undefine default
```

Now to create a full network bridge so that VM's get a LAN IP.

---

SO EASY. Ignore EVERYTHING online, it's this simple:

Works for Ethernet only, not supported on Wi-Fi - might be possible though if I can figure it out, as I'm sure it does work on Windows.

```sh
sudo nmcli connection add ifname br0 type bridge con-name br0
sudo nmcli connection add type bridge-slave ifname enp0s31f6 master br0
nmcli conn show
```

`nmcli` will create two files in `/etc/NetworkManager/system-connections/`.

```sh
-rw-------. 1 root root 218 Oct 27 15:56 br0.nmconnection
-rw-------. 1 root root 212 Oct 27 15:56 bridge-slave-enp0s31f6.nmconnection
```

Contents of `/etc/NetworkManager/system-connections/br0.nmconnection`:

```ini
[connection]
id=br0
uuid=56effead-748d-4ad1-95d1-95f07ae07f09
type=bridge
interface-name=br0
permissions=

[bridge]

[ipv4]
dns-search=
method=auto

[ipv6]
addr-gen-mode=stable-privacy
dns-search=
method=auto

[proxy]
```


Contents of `/etc/NetworkManager/system-connections/bridge-slave-enp0s31f6.nmconnection`:

```ini
[connection]
id=bridge-slave-enp0s31f6
uuid=a963b813-7d2d-4547-9390-59e3986f6a83
type=ethernet
interface-name=enp0s31f6
master=br0
permissions=
slave-type=bridge

[ethernet]
mac-address-blacklist=

[bridge-port]
```

---

Though possible to do a Wi-Fi bridge on Linux, its a lot harder for KVM. Its simply easier to just use VirtualBox. I'm literally only trying to test PXE, and I've waster by whole day faffing around with this. It's good to know its easy for Ethernet as KVM is server focused and that's how I'll be using it, bit its just too messy to do it on my ThinkPad, and ultimately pointless because I want to be able to bridge over Wi-Fi.

So, tomorrow focus on getting PXE working.

---

The state of virtualisation on Linux if you're using VirtualBox is absolutely ridiculous. It just doesn't work, I have absolutely no interest in having to sign kernal modules and having to do that every time Fedora updates the kernel... And, it doesn't even work once you've done it. All I want is to launch a VM to check PXE works - absolute joke.

---

Apparently Ubuntu includes `export LIBVIRT_DEFAULT_URI=qemu:///system` in the /etc/profile.d/libvirt-uri.sh, but on Fedora to be able to run as a non-root user and use //system, add the above export to `.bashrc`

---

Of course on Fedora 35 server. firewalld is running!!! So I need to add a rule to allow access to vnc over port 5901 `nmap 10.19.90.20 -p 5901` will let you check. Just `sudo systemctl stop firewalld` and then check status `sudo firewall-cmd --state`. Really I need to add a firewall rule but that will do for now. More [here](https://docs.fedoraproject.org/en-US/quick-docs/firewalld/)
