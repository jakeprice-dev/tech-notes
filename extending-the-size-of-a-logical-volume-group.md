---
id: 20220930092342
uuid: e069794e-3987-4d6a-8036-ad2efbc0121a
title: Extending the Size of a Logical Volume Group
date: 2022-09-30 09:23:42
modified: 
types: tech-note
categories: linux
link: 
pinned: false
tags: [lvm, logical-volume-group, partition, fedora, disk]
private: false
draft: false
---

I had a situation where after installing a fresh install of Fedora from the Netinstall ISO, when starting a Docker container I was told I had ran out of disk space. My initial reaction was to just reinstall and manually partition instead of having Fedora do it for me in the setup.

However, that meant having to bring my server back upstairs, and I didn't want to do that, so I looked for another route, which turned out to be really easy.

Using `lsblk` you can see how little space was left on `/dev/sda3`.

```sh
$ lsblk -f
NAME                   FSTYPE      FSVER    LABEL  UUID                                   FSAVAIL FSUSE% MOUNTPOINTS
sda
├─sda1                 vfat        FAT32           8FA9-0D2C                               592.6M     1% /boot/efi
├─sda2                 xfs                         9eb5eb28-d901-4f17-aedc-38919304642f    805.9M    21% /boot
└─sda3                 LVM2_member LVM2 001        LirRYx-Vzxw-ctQe-9Nm1-1f3H-6N1a-A3WJU2
  └─fedora_fedora-root xfs                         72dd0d5e-28c9-49b9-a533-d22ce8090098      2.9G    81% /
sdb
└─sdb1                 ext4        1.0      data   57cc0091-0ba2-48f0-a327-cd7c2c4a0545    294.2G    28% /mnt/data-ssd-01
sdc
└─sdc1                 ext4        1.0      backup ebc7b754-fa8f-4387-a979-a55f72a180e0
zram0                                                                                                    [SWAP]
```

As I had Fedora setup paitions with a logical volume group, I was able to check that the volume group had a lot more space capacity (under `VFree`).

```sh
$ sudo vgs
  VG            #PV #LV #SN Attr   VSize    VFree
  fedora_fedora   1   1   0 wz--n- <231.30g <216.30g
```

I then needed to double check the name of my logical volume group `LV Path`, so I could pass it to the command that would extend the group.

```sh
$ sudo lvdisplay -m
  --- Logical volume ---
  LV Path                /dev/fedora_fedora/root
  LV Name                root
  VG Name                fedora_fedora
  LV UUID                MM7KyS-LrL8-umvb-Mc5i-WkRf-WglD-ZvAwlO
  LV Write Access        read/write
  LV Creation host, time main-01, 2022-09-26 17:53:14 +0100
  LV Status              available
  # open                 1
  LV Size                15.00 GiB
  Current LE             3840
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  --- Segments ---
  Logical extents 0 to 3839:
    Type                linear
    Physical volume     /dev/sda3
    Physical extents    0 to 3839
```

This simple command tells the system to increase the volume group capacity by 100% of the remaining capacity.

```sh
$ sudo lvextend -r -l +100%FREE /dev/fedora_fedora/root
```

You can then see that everything was sorted.

```sh
$ lsblk -f
NAME                   FSTYPE      FSVER    LABEL  UUID                                   FSAVAIL FSUSE% MOUNTPOINTS
sda
├─sda1                 vfat        FAT32           8FA9-0D2C                               592.6M     1% /boot/efi
├─sda2                 xfs                         9eb5eb28-d901-4f17-aedc-38919304642f    805.9M    21% /boot
└─sda3                 LVM2_member LVM2 001        LirRYx-Vzxw-ctQe-9Nm1-1f3H-6N1a-A3WJU2
  └─fedora_fedora-root xfs                         72dd0d5e-28c9-49b9-a533-d22ce8090098    210.6G     9% /
sdb
└─sdb1                 ext4        1.0      data   57cc0091-0ba2-48f0-a327-cd7c2c4a0545    294.2G    28% /mnt/data-ssd-01
sdc
└─sdc1                 ext4        1.0      backup ebc7b754-fa8f-4387-a979-a55f72a180e0
zram0                                                                                                    [SWAP]
```
