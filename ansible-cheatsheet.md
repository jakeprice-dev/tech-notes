---
id: 20210116130743
uuid: 5ac82d2b-905d-4697-8cda-12b598d2a193
title: Ansible Cheatsheet
date: 2021-01-16 13:07:43
modified: 
types: tech-note
categories: ansible
pinned: false
tags: [ansible, cheatsheet]
private: false
draft: false
---

## Ansible Roles

```sh
# New Role:
ansible-galaxy role init <role-name>
```

## Ansible Playbooks

```sh
# Run Playbook with Extra Variables:
ansible-playbook --extra-vars host_group=<host-name> <playbook>.yml
```
