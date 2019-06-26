---
id: 20190626150800
uuid: 3481ba7c-a881-49ba-944c-d150b564071c
title: Convert Putty Keys
date: 2019-06-26 15:08:00
modified: 
types: tech-note
categories: putty
pinned: false
tags: [putty, ppk, pem, ssh]
private: false
draft: false
---

To convert a PuTTY `.ppk` key to something that you can use on Linux or WSL start by installing "PuTTY Tools".

```sh
# Fedora
sudo dnf install putty

# Ubuntu
sudo apt install putty-tools
```

Then you can use the `puttygen` utility to convert to `.pem`.

```sh
# Private
puttygen <filename>.ppk -O private-openssh -o <filename>.pem

# Public
puttygen <filename>.ppk -O public-openssh -o <filename>.pem
```

If you need to convert the other way (to `.pem`), you can do that as well.

```sh
# Private
puttygen <filename>.pem -o <filename>.ppk -O private

# Public
puttygen <filename>.pem -o <filename>.ppk -O public
```
