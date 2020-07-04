---
id: 20200704190110
uuid: 531b99ad-4b09-4b36-88f3-ca9a7d93a48d
title: tmux Split Window Shell Alias
date: 2020-07-04 19:01:10
modified: 
types: tech-note
categories: tmux
pinned: false
tags: [tmux, vim, shell, alias]
private: false
draft: false
---

This is a super handy shell alias I found on Reddit.

> Just came up with the simplest shell alias today. I often find myself wanting to edit a file quickly, but also wanting to keep my current shell open. So I created this little alias:
>
> `alias e='tmux split-window -h vim $@'`
>
> So you can do:
>
> `e some-file`
>
> .. to start editing the file without closing your current shell, where the pane will be closed as soon as you exit your editor. Just replace vim with your favorite editor, obviously.
>
> -- [Opening an editor in a new pane for quick editing : tmux](https://www.reddit.com/r/tmux/comments/hfn4h6/opening_an_editor_in_a_new_pane_for_quick_editing/)

I've employed `edit` instead of `e`. With Vim, once you've opened the Vim split you can use the `fzf` plugin to search for a file and open it in that window. That means if you don't want to enter the path you can quickly open the split, and _then_ find the file with `fzf`.
