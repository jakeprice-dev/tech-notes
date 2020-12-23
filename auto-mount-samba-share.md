---
id: 20201223220740
uuid: 9611c17b-680f-416d-8509-ae89c2430a64
title: Automatically Mount Samba Share
date: 2020-12-23 22:07:40
modified: 
types: tech-note
categories: samba
pinned: false
tags: [fstab, samba, cifs]
private: false
draft: false
---

{{< admonition tip >}}
If you encounter issues, or need to query what certain comma separated parameters mean then query the manual: `man mount.cifs`.
{{< /admonition >}}

Create a directory to mount Samba into `mkdir --parents /mnt/my`.

## Fedora 33 KDE and Kubuntu 20.10

Add the below to `/etc/fstab`.

```sh
//<hostname>/my /mnt/my cifs uid=<username>,gid=users,vers=3.1.1,file_mode=0664,dir_mode=0775,x-systemd.automount,noperm,credentials=/home/<username>/.samba,user 0 0
```

The credentials file `$HOME/.samba` should contain credentials as below - set permissions to read-only.

```text
username=value
password=value
```

{{< admonition tip >}}
You may need to install `cifs-utils`.
{{< /admonition >}}

You can test the drive mounts at boot, without rebooting, by running the below.

```sh
sudo umount -f /mnt/my
sudo systemctl daemon-reload
sudo systemctl restart remote-fs.target
```

You probably also need to run the below on Fedora, although it's all a little bit vague.

```sh
sudo chmod +s /bin/mount
sudo chmod +s /bin/umount
sudo chmod +s /usr/sbin/mount.cifs
```

{{< admonition warning >}}
This works, but does seem to have trouble automounting and auto-unmounting on Fedora. I've not worked out why that is yet. I often have to run `sudo mount -a` after logging in.
{{< /admonition >}}
