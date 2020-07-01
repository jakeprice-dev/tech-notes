---
id: 20200701192231
uuid: ea329c8e-0e43-4583-920a-5e711c44fdb0
title: fzf Search on My Notes
date: 2020-07-01 19:22:31
modified: 
types: tech-note
categories: cli
pinned: false
tags: [fzf, search, notes]
private: false
draft: false
---

I've finally found out how you can use `fzf` in a much better way, that allows me to search for strings within files, but also display a preview of that file.

Previously to search within files I piped `grep` into `fzf` but the trouble was that `fzf` would read the whole line, and not decipher just the filename, so opening up my note search you would see a line like the below, and `fzf`'s previewer would read the whole line as the filename, and obviously fail to open a preview.

```sh
/home/jprice/my/documents/notes/20200516211955.md:# Git Commit Tags
```

All I have to do was read the manual! Which states:

> `--preview=COMMAND` - Execute the given command for the current line and display the result on the preview window. **`{}` in the command is the placeholder that is replaced to the single-quoted string of the current line. To transform the replacement string, specify field index expressions between the braces** (See FIELD INDEX EXPRESSION for the details).

It turns out, using a thing called "Field Index Expressions" I could do something like `--preview 'bat {1}'` and it would select the first string, which is the filename, so that meant I could now preview the file name properly! I could also pass it a delimiter with `--delimiter ':'` and that allowed me to ensure that the `:` between the filename and the `grep` result wasn't considered as part of the `{1}` selection, so even with the HTML files in my archive the preview would always preview the right filename! Amazing!

The final command is below:

```sh
grep --recursive --exclude-dir={scripts,views} "" $HOME/my/documents/notes/ | sort --reverse | fzf --delimiter ':' --preview "bat --italic-text=always --theme=base16 --style=numbers --color=always {1}"
```
