---
id: 20221116172241
uuid: 6f025e16-41af-47da-853c-384b4ff15ca7
title: Open Files Side-By-Side with Vim 
date: 2022-11-16 17:22:41
modified: 
types: tech-note
categories: vim
link: 
pinned: false
tags: [vim, split, open, file]
private: false
draft: false
---

I'll often want to open a couple of files in Vim and then split the main window so I have one file on the left, and one file on the right. I've just learnt that you can tell Vim to open each in it's own split, a vertical split or a horizontal split.

```sh
# Vertical split:
vim -O <file1> <file2>

# Horizontal split:
vim -o <file1> <file2>
```

It works with more than one file as well, so you could open three files, and have a pane for each.

