---
id: 20220208101809
uuid: ed824946-c622-4bd0-88e9-aac9e0f2cda0
title: Resize Logical Volume Partition
date: 2022-02-08 10:18:09
modified: 
types: tech-note
categories: linux
link: 
pinned: false
tags: [lvm, qemu, vm, partition, virsh]
private: false
draft: false
---

```sh
ERROR: failed to register layer: Error processing tar file(exit status 1): write /usr/local/share/.config/yarn/global/node_modules/code-server/vendor/modules/code-oss-dev/out/vs/workbench/workbench.web.api.js: no space left on device
```

Check available space:

```sh
$ df -h
Filesystem                         Size  Used Avail Use% Mounted on                                   
udev                               972M     0  972M   0% /dev                                         
tmpfs                              199M  1.7M  197M   1% /run                                         
/dev/mapper/ubuntu--vg-ubuntu--lv   19G   18G  152M 100% /                                            
tmpfs                              992M     0  992M   0% /dev/shm                                     
tmpfs                              5.0M     0  5.0M   0% /run/lock                                    
tmpfs                              992M     0  992M   0% /sys/fs/cgroup                               
/dev/vda2                          976M  401M  508M  45% /boot                                        
mount_my                           458G   84G  351G  20% /mnt/my                                      
tmpfs                              199M     0  199M   0% /run/user/1000
```

Shutdown virtual machine.

```sh
virsh shutdown <vm-name>
```

Get location of disk.

```sh
virsh domblklist ppn-int-gb1-vs-services-01
```

Resize the disk.

```sh
sudo qemu-img resize /srv/data-ssd-01/vm-data/ppn-int-gb1-vs-services-01.qcow2 +10G
```

Restart the virtual machine.

```sh
virsh start ppn-int-gb1-vs-services-01
```

SSH onto the virtual machine, and make sure `cloud-guest-utils` is installed.

```sh
sudo apt install cloud-guest-utils
```

Rewrite the partition table to take up the now free available space.

```sh
sudo growpart /dev/vda 3
CHANGED: partition=3 start=2101248 old: size=39839744 end=41940992 new: size=60813279 end=62914527
```

Resize the volume.

```sh
sudo pvresize /dev/vda3
Physical volume "/dev/vda3" changed
  1 physical volume(s) resized or updated / 0 physical volume(s) not resized 
```

Get the logical volume path.

```sh
sudo lvdisplay 
  --- Logical volume ---
  LV Path                /dev/ubuntu-vg/ubuntu-lv                                                     
  LV Name                ubuntu-lv                                                                    
  VG Name                ubuntu-vg                                                                    
  LV UUID                UBfHbd-yfXr-aNS9-d2yu-qA3W-1QuN-jdLDJJ
  LV Write Access        read/write                                                                   
  LV Creation host, time ubuntu-server, 2021-11-20 10:34:28 +0000
  LV Status              available                                                                    
  # open                 1                                                                            
  LV Size                <19.00 GiB                                                                   
  Current LE             4863                                                                         
  Segments               1                                                                            
  Allocation             inherit                                                                      
  Read ahead sectors     auto                                                                         
  - currently set to     256                                                                          
  Block device           253:0
```

Then finally extend the logical volume.

```sh
sudo lvextend -r -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
  Size of logical volume ubuntu-vg/ubuntu-lv changed from <19.00 GiB (4863 extents) to <29.00 GiB (742
3 extents).
  Logical volume ubuntu-vg/ubuntu-lv successfully resized.
resize2fs 1.45.5 (07-Jan-2020)                                                                        
Filesystem at /dev/mapper/ubuntu--vg-ubuntu--lv is mounted on /; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 4                                                              
The filesystem on /dev/mapper/ubuntu--vg-ubuntu--lv is now 7601152 (4k) blocks long.
```

You should now see that the volume has increased in size.

```sh
df -h
Filesystem                         Size  Used Avail Use% Mounted on
udev                               1.4G     0  1.4G   0% /dev
tmpfs                              293M  1.6M  291M   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv   29G   18G  9.7G  65% /
tmpfs                              1.5G     0  1.5G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
tmpfs                              1.5G     0  1.5G   0% /sys/fs/cgroup
/dev/vda2                          976M  401M  508M  45% /boot
mount_my                           458G   84G  351G  20% /mnt/my
tmpfs                              293M     0  293M   0% /run/user/1000
```
