---
id: 20200326190425
uuid: 1334ac64-e2c7-4c67-8feb-a9e4d48a7fd3
title: Ctrl-A and DuckDuckGo Fedora Bug
date: 2020-03-26 19:04:25
modified: 
types: tech-note
categories: fedora
pinned: false
tags: [shortcuts, keyboard, duckduckgo, fedora]
private: false
draft: false
---

When trying to "select all" using `Ctrl` + `A` on [DuckDuckGo](https://duckduckgo.com), the whole page is selected instead of the contents of the search input field.

It's caused by having `Pointer Location` turned on in GNOME:

> It seems `/usr/libexec/gsd-locate-pointer`, when invoked under fvwm without session
> manager, will enable "show pointer location when pressing Ctrl key"
> even when the underlying value of the dconf/gsettings key is false.
> This interferes duckduckgo's search box, making key shortcuts with Ctrl
> prefix (e.g. Ctrl+C for copy) unusable. The issue is gone after we stop invoking this gsd plugin. 
>
> -- https://www.reddit.com/r/duckduckgo/comments/a4s35t/cannot_use_ctrl_prefix_key_shortcuts_in/
