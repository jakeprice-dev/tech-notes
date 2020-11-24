---
id: 20201124093331
uuid: 3e3063ea-f3ce-409f-a1f1-57d7ab85b35f
title: Create an SFTP Server
date: 2020-11-24 09:33:31
modified: 
types: tech-note
categories: self-hosted
pinned: false
tags: [sftp, linux, password]
private: false
draft: false
---

At work I've used a couple of options for enabling SFTP. The directory structure I tend to use is as follows.

```text
/srv/
└── <company>
    └── <product>
        └── <env_name>
            ├── inbound
            ├── outbound
            └── .ssh
                └── authorized_keys
```

This is configured to use Port 2222 for SFTP, and _not_ Port 22. In fact Port 22 is blocked for SFTP access.

## Initial SSHD Config Changes

Initially the following gets added to the top of `/etc/ssh/sshd_config`.

```ini
Port 22
Port 2222
AuthorizedKeysFile .ssh/authorized_keys
```

Additionally you need to comment out this line (typically on line 141).

```ini
Subsystem       sftp    /usr/libexec/openssh/sftp-server
```

## SFTP with a Password

This is the configuration I use for password only authentication, with port 2222 allowed, and 22 denied.

```ini
# Switch to internal-sftp, as newer:
Subsystem sftp internal-sftp

# Match block:
Match Group sftp LocalPort 2222
    ChrootDirectory /srv/<company>/<product>
    ForceCommand internal-sftp
    AllowTCPForwarding no
    X11Forwarding no
    PasswordAuthentication yes
    AuthenticationMethods password

Match Group sftp LocalPort 22
    DenyGroups sftp
```

I then set the password for the user I wish to give access to.

```sh
sudo passwd sftp_user
```

## SFTP with a Password and Private Key

Whereas this is the configuration for password and private key.

```ini
# Switch to internal-sftp, as newer:
Subsystem sftp internal-sftp

# Match block:
Match Group sftp LocalPort 2222
    ChrootDirectory /srv/<company>/<product>
    ForceCommand internal-sftp
    AllowTCPForwarding no
    X11Forwarding no
    PasswordAuthentication yes
    AuthenticationMethods password,publickey

Match Group sftp LocalPort 22
    DenyGroups sftp
```

## Users

You then need to add an SFTP group and user, and give permissions to the directories you are serving via SFTP.

```sh
# Add SFTP group and user:
groupadd sftp
useradd <sftp_user> --groups sftp --shell /sbin/nologin --home-dir /srv/<company>/<product>/<sftp_user>

# Set password for sftp user:
echo <sftp_user>:<sftp_user_password> | chpasswd

# SFTP directory permissions:
chown --recursive <sftp_user>:<sftp_user> /srv/<company>/<product>/<sftp_user>
chmod --recursive 755 /srv/<company>/<product>/<sftp_user>/

# Comment out Subsystem sftp /usr/libexec/openssh/sftp-server
sed -i.bak '141s/^/#/' /etc/ssh/sshd_config
sed -i.bak 's/#Port 22/Port 22\nPort 2222/' /etc/ssh/sshd_config
```

## Key Pair

Add the keypair public key you will use to connect to `/srv/<company>/<product>/<sftp_username>/.ssh/authorized_keys`.

## SELinux

If you're running SELinux, you then need to tell it about the SFTP Port, and stop it from blocking read access to the `authorized_keys` file.

```sh
# Tell SELinux about SFTP Port:
sudo semanage port --add --type ssh_port_t --proto tcp 2222

# Stop SELinux from blocking read access to authorized_keys:
semanage fcontext -a -t ssh_home_t '/srv/<company>/<product>/<sftp_server_username>/.ssh/authorized_keys'
restorecon -v '/srv/<company>/<product>/<sftp_server_username>/.ssh/authorized_keys'
```

## Restart SSH

You can then restart the SSH server and test if you can connect.

```sh
systemctl restart sshd.service
```
