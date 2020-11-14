---
id: 20201114155833
uuid: 7d36541f-8e03-47b3-965f-5bcc4f43ca87
title: Mouse Scroll Escape Codes in Terminal
date: 2020-11-14 15:58:33
modified: 
types: tech-note
categories: cli
pinned: false
tags: [mouse, scrolling, escape, terminal, shell]
private: false
draft: false
---

Very rarely I lose the ability to scroll in a terminal window, and when I try, the escape code `65;70;7M` is printed constantly instead.

There's a quick way to fix this, and that's to simply run `reset` to reset the terminal window, and you'll be back to normal.

More can be read here: [Scrolling is disabled all of a sudden in terminal - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/259922/scrolling-is-disabled-all-of-a-sudden-in-terminal)
