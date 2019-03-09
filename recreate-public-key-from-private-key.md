---
id: 20190309200745
uuid: 0e90b140-9245-4fef-bc5f-982a70e1f1d8
title: Recreate Public Key from Private Key
date: 2019-03-09 20:07:45
modified: 
types: tech-note
categories: ssh
pinned: false
tags: [ssh, ssh-keygen, public-key, private-key]
private: false
draft: false
---

If you've lost a public key you can read the private key and have `ssh-keygen` recreate the public key.

```sh
$ ssh-keygen -y -f <private-key-file> > <public-key-file>.pub
```

It won't retain the private key's comment so you will need to manually append a comment to the public key if required.
