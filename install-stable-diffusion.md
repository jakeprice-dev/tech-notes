---
id: 20220912220125
uuid: 531fd140-1d69-4869-83ec-424cffde3700
title: Install InvokeAI Stable Diffusion Fork
date: 2022-09-12 22:01:25
modified: 
types: tech-note
categories: miscellaneous
link: https://github.com/lstein/stable-diffusion/blob/main/docs/installation/INSTALL_LINUX.md
pinned: false
tags: [stable-diffusion, nvidia, gpu, cuda, conda, ai, art, machine-learning]
private: false
draft: false
---

Follow steps [here](https://github.com/lstein/stable-diffusion/blob/main/docs/installation/INSTALL_LINUX.md) but it's easier to run the Anaconda install with the `-b` flag as below.

```sh
$ ./Anaconda3-2022.05-Linux-x86_64.sh -b
```

Furthermore, with Anaconda we don't want it to be activated on login, so disable that with the below command:

```sh
$ conda config --set auto_activate_base false
```

