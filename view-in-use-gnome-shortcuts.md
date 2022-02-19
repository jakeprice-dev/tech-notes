---
id: 20220219163400
uuid: 3e0b5628-63dc-42ab-acd4-26fbb621628b
title: View All Gnome Shortcuts
date: 2022-02-19 16:34:00
modified: 
types: tech-note
categories: gnome
link: https://askubuntu.com/questions/1317701/list-all-gnome-shortcuts-currently-in-use
pinned: false
tags: [gnome, shortcut, gsettings, dconf]
private: false
draft: false
---

To view a list of all shortcuts with their corresponding `gsettings` path, run the below. You can substitute `Super` with other keys obviously.

```sh
gsettings list-recursively | grep \<Super\>
org.gnome.settings-daemon.plugins.media-keys rotate-video-lock-static ['<Super>o', 'XF86RotationLockToggle']
org.gnome.settings-daemon.plugins.media-keys touchpad-toggle-static ['XF86TouchpadToggle', '<Ctrl><Super>XF86TouchpadToggle']
org.gnome.settings-daemon.plugins.media-keys magnifier-zoom-in ['<Alt><Super>equal']
org.gnome.settings-daemon.plugins.media-keys magnifier-zoom-out ['<Alt><Super>minus']
org.gnome.settings-daemon.plugins.media-keys help ['', '<Super>F1']
org.gnome.settings-daemon.plugins.media-keys screenreader ['<Alt><Super>s']
org.gnome.settings-daemon.plugins.media-keys magnifier ['<Alt><Super>8']
org.gnome.settings-daemon.plugins.media-keys screensaver ['<Super>l']
org.gnome.desktop.wm.keybindings switch-group ['<Super>Above_Tab', '<Alt>Above_Tab']
```


