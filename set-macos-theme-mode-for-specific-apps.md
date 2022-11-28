---
id: 20221128151334
uuid: 2d1477a9-e206-401b-a073-9a3efdbc6c6e
title: Set macOS Theme for Specific Apps
date: 2022-11-28 15:13:34
modified: 
types: tech-note
categories: macos
link: 
pinned: false
tags: [macos, theme, dark-mode]
private: false
draft: false
---

```sh
osascript -e 'id of app "Outlook"'

com.microsoft.Outlook

# Exclude from dark mode:
defaults write <Bundle-Identifier> NSRequiresAquaSystemAppearance -bool yes

# Dark mode on:
defaults write com.microsoft.Outlook NSRequiresAquaSystemAppearance -bool no

# Reset mode:
defaults delete <Bundle-Identifier> NSRequiresAquaSystemAppearance
```
