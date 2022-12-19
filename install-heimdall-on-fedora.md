---
id: 20221219130830
uuid: 81d8b312-b319-45f5-9053-a5d31ed47158
title: Install Heimdall on Fedora
date: 2022-12-19 13:08:30
modified: 
types: tech-note
categories: android
link: https://github.com/Benjamin-Dobell/Heimdall/wiki/Build-instructions-(Linux)#fedora
pinned: false
tags: [android, heimdall, linux, odin, samsung, flash, image, firmware]
private: false
draft: false
---

{{<admonition warning>}}
Doing this is all well and good, but it's impossible to use - because there's no documentation on how to map the partitions to the files you should upload! Unfortunately Odin on Windows remains the simplest way to flash firmware for Samsung devices.
{{</admonition>}}


## Prerequisites

```sh
sudo dnf groupinstall "Development Tools"
sudo dnf install libusb1-devel qt-devel libtool
sudo dnf install cmake glibc libusb zlib gcc qt5-qtbase-devel
```

## Build

```sh
git clone git@github.com:Benjamin-Dobell/Heimdall.git
cd Heimdall
mkdir -p build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make
sudo cp bin/* /usr/local/bin
```

## Run

Start Heimdall by running `heimdall` or `heimdall-frontend` from the terminal.
