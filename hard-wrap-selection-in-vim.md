---
id: 20221021123711
uuid: ef3e44ea-1476-43a2-8df0-e1d915099446
title: Hard Wrap Selection in Vim
date: 2022-10-21 12:37:10
modified: 
types: tech-note
categories: vim
link: 
pinned: false
tags: [vim, hard-wrap, wrap, text-width]
private: false
draft: false
---

This is a handy little trick to hard wrap a selection of text to a specific column number. I usually use this when I'm rewriting a Git commit message which I may have edited a bit and lost the automatic hardwrapping Vim enables for Git commit messages.

Here's an example message.

```text
Change `entries` command to only list by default

`entries` will not prompt for an ID to edit after displaying the list, `--edit`, or `-e` can now be appended to prompt for an ID to edit in the
default editor.
```

Tell Vim the `textwidth` you want to hardwrap to:

```sh
:set textwidth=72
```

Then select the text you wish to wrap, and type the following key sequence.

```sh
gq$
```

You'll then get a perfectly hardwrapped message as can be seen below:

```text
Change `entries` command to only list by default

`entries` will not prompt for an ID to edit after displaying the list,
`--edit`, or `-e` can now be appended to prompt for an ID to edit in the
default editor.
```
