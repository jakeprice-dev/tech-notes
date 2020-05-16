---
id: 20200516104632
uuid: 73da8d1e-66ed-41af-bfd0-5985bd117e70
title: Pandoc Cheatsheet
date: 2020-05-16 10:46:32
modified: 
types: tech-note
categories: pandoc
pinned: false
tags: [pandoc, commands, markdown, markup, cheatsheet]
private: false
draft: false
---

```sh
# Convert Markdown to HTML:
pandoc --from=markdown --to=html <file>.md > <file>.html

# Create a self-contained HTML file:
pandoc --toc --css <stylesheet>.css --standalone <input>.md --toc --output <output>.html
```
