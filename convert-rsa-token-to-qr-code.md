---
id: 20210812163922
uuid: be1aaf29-d010-4952-8e99-c0903875d9fd
title: RSA Token to QR Code
date: 2021-08-12 16:39:22
updated: 
types: tech-note
categories: cli
pinned: false
tags: [rsa, token, qr, logical-access]
private: false
draft: false
---

At a previous company I worked at, I used `qrencode` to encode an RSA Token link on my work MacBook.

Make sure you install  `brew install qrencode`.

Then run:

```sh
qrencode -t ansiutf8 http://127.0.0.1/securid/ctf?ctfData=1399503853395794776958778006207323342964422284215023231274725406514288673546241243
```

That will output a QR code on the terminal screen and you can scan it in the RSA app.
