---
id: 20220204115426
uuid: faf3cb58-4fd5-4642-8f90-c6cb472e65c0
title: Send Output to Clipboard using xclip
date: 2022-02-04 11:54:27
modified: 
types: tech-note
categories: cli
link: 
pinned: false
tags: [xclip, xsel, clipboard, stdout, stdin, copy, paste]
private: false
draft: false
---

To send the output of a command in the terminal to the clipboard, the below is super helpful.

```sh
xclip -selection clip /tmp/owm.json
```

