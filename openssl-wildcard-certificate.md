---
id: 20210109204554
uuid: ec023a37-5b4b-4b4b-9b75-567e8d154576
title: Wildcard OpenSSL Certificate Authority
date: 2021-01-09 20:45:54
modified: 
types: tech-note
categories: openssl
pinned: false
tags: [openssl, lan, self-hosted, certificate, authority]
private: false
draft: false
---

{{<toc>}}

I deploy a really simple certificate authority for all my self-hosted, LAN services, which deploys a single root authority, and a wildcard certificate.

## Directory Structure

```sh
mkdir --parents ~/my/files/code/config/ark-ca/{certificates,private-keys}
```

The structure will end up looking like this when certificates and keys have been created.

```text
├── certificates
│   ├── ark_lan_cert.pem
│   ├── ark_lan_csr.pem
│   ├── ark_root_ca_cert.pem
│   └── ark_root_ca_cert.srl
├── private-keys
│   ├── ark_lan_key.pem
│   └── ark_root_ca_key.pem
└── v3.ext
```

## Root CA

### Create Root Key and Certificate

```sh
openssl req \
    -x509 \
    -newkey rsa:4096 \
    -keyout ~/my/files/code/config/ark-ca/private-keys/ark_root_ca_key.pem \
    -nodes \
    -days 10950 \
    -sha256 \
    -extensions v3_ca \
    -subj "/C=GB/ST=LAN/L=LAN/O=Ark Certificate Authority/CN=Ark Root CA" \
    -out ~/my/files/code/config/ark-ca/certificates/ark_root_ca_cert.pem
```

## Server Certificate

### Create Key & Certificate Signing Request

```sh
openssl req \
    -new \
    -newkey rsa:4096 \
    -nodes \
    -keyout ~/my/files/code/config/ark-ca/private-keys/ark_lan_key.pem \
    -subj "/C=GB/ST=LAN/L=LAN/O=Ark LAN/CN=*.ark.lan" \
    -out ~/my/files/code/config/ark-ca/certificates/ark_lan_csr.pem
```

Check the Certificate Signing Request.

```sh
openssl req \
    -text \
    -noout \
    -verify \
    -in ~/my/files/code/config/ark-ca/certificates/ark_lan_csr.pem
```

### Create Extensions

Create and add the below content into `~/my/files/code/config/ark-ca/v3.ext`.

```ini
[req]
req_extensions = v3_req

[ v3_req ]
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = *.ark.lan
DNS.2 = ark.lan

```

### Create the Certificate

```sh
openssl x509 \
    -req \
    -in ~/my/files/code/config/ark-ca/certificates/ark_lan_csr.pem \
    -extfile ~/my/files/code/config/ark-ca/v3.ext \
    -extensions v3_req \
    -CA ~/my/files/code/config/ark-ca/certificates/ark_root_ca_cert.pem \
    -CAkey ~/my/files/code/config/ark-ca/private-keys/ark_root_ca_key.pem \
    -CAcreateserial \
    -out ~/my/files/code/config/ark-ca/certificates/ark_lan_cert.pem \
    -days 365 \
    -sha256
```

Check the certificate looks correct and includes everything you want it to include.

```sh
openssl x509 \
    -noout \
    -text \
    -in ~/my/files/code/config/ark-ca/certificates/ark_lan_cert.pem
```
