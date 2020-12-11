---
id: 20201211133432
uuid: e1e2626d-df78-4123-92ee-7da77940492b
title: Prepare a New Hard Drive in Linux
date: 2020-12-11 13:34:32
modified: 
updpated: 
types: tech-note
categories: linux
pinned: false
tags: [format, ext4, hard-drive, linux, disk]
private: false
draft: false
---

## Identify the New Drive

Make sure the drive is installed by using `lsblk` to check. In the example below, it's `sdb`.

```sh
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 238.5G  0 disk
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0   237G  0 part /
└─sda3   8:3    0   977M  0 part [SWAP]
sdb      8:16   0 931.5G  0 disk
```

## Create Partition

```sh
sudo parted /dev/sdb mklabel gpt
sudo parted --align optimal /dev/sdb mkpart primary ext4 0% 100%
```

If you type `lsblk` again, you'll now see the partition.

```sh
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 238.5G  0 disk
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0   237G  0 part /
└─sda3   8:3    0   977M  0 part [SWAP]
sdb      8:16   0 931.5G  0 disk
└─sdb1   8:17   0 931.5G  0 part
```

## Create a Filesystem on The Partition

```sh
sudo mkfs --type ext4 -L backup /dev/sdb1
```

All being well the filesystem will have been written to the disk without issue.

```sh
mke2fs 1.44.5 (15-Dec-2018)
Creating filesystem with 244190208 4k blocks and 61054976 inodes
Filesystem UUID: ebc7b754-fa8f-4387-a979-a55f72a180e0
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
        102400000, 214990848

Allocating group tables: done
Writing inode tables: done
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done
```

Let's check everything is looking ok with `sudo lsblk -o NAME,FSTYPE,LABEL,UUID,MOUNTPOINT`

```sh
NAME   FSTYPE LABEL  UUID                                 MOUNTPOINT
sda
├─sda1 vfat          8D94-4DC4                            /boot/efi
├─sda2 ext4          4f6f8b2f-46c7-434b-9c67-4933ef359370 /
└─sda3 swap          db9aa753-c969-469c-8573-5c2de77f42fa [SWAP]
sdb
└─sdb1 ext4   backup ebc7b754-fa8f-4387-a979-a55f72a180e0
```

## Mount the Filesystem

```sh
sudo mkdir --parents /mnt/backup
sudo mount --options defaults /dev/sdb1 /mnt/backup
```

Finally to ensure the drive is mounted on startup, we'll append the below to `/etc/fstab`.

```sh
UUID=<uuid_here> /mnt/backup ext4 defaults 0 2
```
