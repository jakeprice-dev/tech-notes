---
id: 20210101150358
uuid: 7749921f-4855-4c8c-aca2-248aa4ad0f18
title: Install Fonts on Linux
date: 2021-01-01 15:03:58
modified: 
types: tech-note
categories: linux
pinned: false
tags: [linux, fonts]
private: false
draft: false
---

Install fonts using the Terminal on Linux.

Using _Inconsolata_ as an example.

```sh
wget https://github.com/googlefonts/Inconsolata/releases/download/v3.000/fonts_ttf.zip --output-document=/tmp/inconsolata_v3.zip
sudo mkdir --parents /usr/local/share/fonts
sudo cp /tmp/fonts/ttf/Inconsolata-*.ttf /usr/local/share/fonts/
sudo fc-cache /usr/local/share/fonts/
```

{{< admonition tip >}}
You can check `/etc/fonts/fonts.conf` for a list of directories that contain fonts.
{{< /admonition >}}
