---
id: 20200619100544
uuid: d6b002c3-e43a-4a60-b124-f93f244f9ea0
title: Edit over SSH with Vim
date: 2020-06-19 10:05:44
modified: 
types: tech-note
categories: vim
pinned: false
tags: [vim, remote, scp]
private: false
draft: false
---

Vim can edit files remotely, on a server. It uses `scp` to copy it locally, allow you to work on it, and then it copies it back to the server. You can even use `netrw` to view remote directories, and then select a file to edit.

Firstly you need to add a host to your ssh `config` file:

```sh
Host <name>
    HostName <ip>
    User <username>
    IdentityFile <ssh-key>
```

Then you can simply edit as below:

```sh
# Directory:
vim scp://<name>//home/<username>/

# File:
vim scp://<name>//home/<username>/<file>.py
```

If you have an issue with the statusbar not displaying properly you can just refresh the screen with `^L`.
