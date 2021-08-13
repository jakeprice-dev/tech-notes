---
id: 20210813161121
uuid: c66b3384-3b78-43c7-b426-c7ae5ce99715
title: OpenSSL on MacOS
date: 2021-08-13 16:11:21
modified: 
types: tech-note
categories: openssl
pinned: false
tags: [openssl, macos]
private: false
draft: false
---

`/etc/ssl/openssl.cnf` should have the below by default on Fedora at least, but on MacOS it doesn't and has to be added.

```ini
[ v3_ca ]
basicConstraints = critical,CA:TRUE
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer:always
```

Also, append:

```ini
x509_extensions = v3_ca
```

To the end of `[ req ]`.

