---
id: 20200826163942
uuid: 8292c009-7e4a-472d-b420-46f5ff40a07b
title: OpenSSL RSA Encryption
date: 2020-08-26 16:39:42
modified: 
types: tech-note
categories: openssl
pinned: false
tags: [openssl, rsa, encryption, keys, private, public]
private: false
draft: false
---

More can be found here: [/docs/man1.1.1/man1/rsautl.html](https://www.openssl.org/docs/man1.1.1/man1/rsautl.html)

## Create a Key Pair

### Create a Private Key

```sh
openssl genrsa -out rsa.private 2096
```

That will create a private key as below.

```text
→ cat rsa.private
-----BEGIN RSA PRIVATE KEY-----
MIIEwAIBAAKCAQcAuyGLukkViova3tbNQMcgjkeXCNFU6TRExHxXsE4EFNd5SHgE
cwJjfXNYwYNpHhgv9EO1O80NF/vbDQ5eYs1i7PBzE1e9KFGIXYhGvl+jS9Nh01Ip
Ez4JIDsxzOwSqyMRAqNCVS4BMDEEWQFq69bG+BDXPn/MhNLincO6zxaaYhVPUgKw
30EUmPyaDHxuw6GW7MANBueWUyI064z/yUlilOVrWawbuLVK2imbTYHc3iBrJw/g
XtcsbtC936k9IVKAK6rAOAA5k6l2Q2rj8R4APFH+gXkBhmu9W0mG5Se+yd8sl0/a
tMzVQKnaf2L3qa8Xhu+6D/y7UXO0fo0C/X75GT5BRq4VTQIDAQABAoIBBwCw1DU+
Vht17UxosKAKK2C3Mi0llvHe4JZu+XS/VCmFLEweT9McftBW1m2zhyMI3iBKeMTE
erJ5zHOlNcO5J43oihqj15AWjl0GzWTUNnmPiHLzhrrrxP5Ip8cadqX5c8x6Kp3e
8fcLe/Y4yR8pNq6T8Q4EsC0qAkFEhZV8ZUaDktF+kw6KsxYSV4X3+lXIY1PAesmR
ZJtoaQ2ZVg+7g3jKHep94/ZVkzyLzAdxnMZuPywFS92a15jBhbdKIf3+ZVZKipmJ
lOZTYZWRWSwQfkWl4/GfTp3S0LTZdtYieqnG9h5Vh0mxASsI90bS4n1A1+FZNxWE
No8biur14R0DGh3pi16UGen9AoGEAPKSBzijA0fkjGPw81Krea3ZvbeZ1WqJqyzG
sUYsUg7d8QrMgy+TRsb0/1Hf4S8QxZMF9zLRRlr2cfV55flPYyvpyrQzM/sgnzz7
WA15D3jPHt3iSASYimb1x4pZij6kbPyC8aouh3EuIu9wRS6KAHBcWPj8MTCXQ+u3
o5S191QGIeAzAoGEAMV9xTaroTqlLEjeKOTrMfvdt0qGK/CBp9kQUDT9VJZm/m7j
oeEC5laKSa/bpRf/UtFvQidWpx3oG19QZxRtCUmKnS4BHSz6pTPLcjsNqTtYggJE
elVyQET2yuifeUF8Qcc/O8t8Nr38CQ59XCCZZE/RhsjCuMro9ZoIAc6ioDhA1LR/
AoGEAPFMFETh2/yjPIiaguIk08j+Bfxi2eq+AfpruKLuqb5CR2qPoJwt7EU0J9uh
UpcIHxIu6AW0KkGIUDp5xSxpLWTcj5kMKlRQ0jbObHwShjKODs14MF3qBBuBuLpG
cf/4frX96Dp7SJ5s37HBxAAUpXUgWB1uedw1TQppxh7DmMILy50DAoGEAInAxZvx
sV9nsPEOzTWH/HBzYZNA7UswFPTqVRfxEFxscNLGQrbhv+rjQKzCp3CKrqjZUyp/
XcgVi2o7efscZxr97c8WBdTG7A6pvP8F82IqxVxxiWcqbzvrbPM/sWOFGZiyObIg
7Uotj9+kf2tZs0edncdRU3ZtckrDhQcuFlXGmtdfAoGDdtFh8/YM0MaTna6/3zxV
SMHc2wr1EBDLEeHbZ55hm7djrJ/d1/G24jwdJXzjsFuCfxJG6PQ3VeqAGY75ttpS
3rUGBlJhwL8sbKcbskfMmgxbzm3fxfXbZ4eWCSAVRd97zQfLv1/CWQyK+UQbmfDz
HEUIF77GycX2GqFJiUh5WKxxPtg=
-----END RSA PRIVATE KEY-----
```

### Create a Public Key

Now create a public key, from the private key.

```sh
openssl rsa -in rsa.private -out rsa.public -pubout -outform PEM
```

That will create a public key as below.

```text
-----BEGIN PUBLIC KEY-----
MIIBKDANBgkqhkiG9w0BAQEFAAOCARUAMIIBEAKCAQcA61Wc4IFbKUj1QXSkcmWA
eIOCPISvM/avrM8itTruqpXTPrYn0idik7DJR1esxQ7wOcALXifhJb/z99mCnL7A
C4DusYLLKOldf4WAtTiU6Uwf3x6+PWdP5UmUVVgpZOo0ztD3xAi7T809evNNMzZ+
pJ2021D387cO2A5vAADfnZi8qUwud/ooRQyGo1BpJqhQFDtI+ko5DnpHK8js5U4w
E9+80A+wzBCJe9g79CpBcPDsJy9+52wBpZdWptVD5nnxWWWKHFQJqsM6nlKFbqiH
t9cVBDlYwPp91itSuPVIrq9xz/tSOarYr5r4B8urZImX1cvkKSVTHfeLSFlp3zjc
B0SKPoJwJwIDAQAB
-----END PUBLIC KEY-----

```

## Encrypt a Message

```sh
echo "Hello Jake!" | openssl rsautl -encrypt -pubin -inkey rsa.public > message.encrypted
```

The `message.encrypted` file will be garbled, because it is now encrypted.

You can also encrypt a file in the same way.

```sh
cat <file_name> | openssl rsautl -encrypt -pubin -inkey rsa.public > message.encrypted
```

## Decrypt a Message

```
openssl rsautl -decrypt -in message.encrypted -inkey rsa.private
```

You'll see the output of the decrypted message after running the command.
