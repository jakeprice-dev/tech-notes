---
id: 20201029211853
uuid: 6aa242db-a857-4144-8014-6e58afbaa33a
title: SELinux Cheatsheet
date: 2020-10-29 21:18:53
modified: 
types: tech-note
categories: selinux
pinned: false
tags: [selinux, centos, redhat, security, ports, blocking, allow, firewall, cheatsheet]
private: false
draft: false
---

SELinux is the cause of a number of issues sometimes, if I can't access a port, and the firewall allows it, on a system using SELinux then it's probably because of SELinux.

The below contains helpful commands that I have come across whilst using SELinux.

- AVC: Access Vector Cache

> A denial is the event generated anytime that a service, application, file, etc. is denied access by the SELinux system. When this happens, the denial is cached in the Access Vector Cache (AVC). You will sometimes see a denial message referred to as an AVC denial.

```sh
# Get the status:
sudo sestatus
sudo getenforce

# Set status to Permissive:
sudo setenforce 0

# Install Setools and Setroubleshoot:
yum install setroubleshoot setools

# View manpage:
man sealert

# Scan the log file for SELinux issues:
sealert --analyze /var/log/audit/audit.log

# View manpage:
man semanage port

# Add and delete a port type:
sudo semanage port --add --type ssh_port_t --proto tcp 2222
sudo semanage port --delete --type ssh_port_t --proto tcp 2222

# Check what the correct context should be:
matchpathcon /var/www/html/
ls -lZ

# Denials are logged in:
# - auditd on - /var/log/audit/audit.log
# - auditd off; rsyslogd on - /var/log/messages
# - setroubleshootd, rsyslogd, and auditd on - Both locations, though the messages in /var/log/messages are easier to make sense of

# Check for problems caused by SELinux:
sudo grep "SELinux is preventing" /var/log/messages
sudo grep "denied"/var/log/audit/audit.log

# List brief description of SELinux booleans:
sudo semanage boolean --list
sudo getsebool <boolean>

# For more detail:
sudo dnf install selinux-policy-devel

# Set boolean to on, and persist:
sudo setsebool -P <boolean> on
```
