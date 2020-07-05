---
id: 20200705194321
uuid: 07dfa8cd-c4d9-4ac7-b4d1-c5d43dca2b62
title: Enable Scope Inspector in VS Code
date: 2020-07-05 19:43:21
modified: 
types: tech-note
categories: vscode
pinned: false
tags: [vscode, editor]
private: false
draft: false
---

Use the scope inspector to debug grammars. You can view scopes for a selected token.

Enable it with `Ctrl-Shift-P` and type in `scope` and select `Developer: Inspect Editor Tokens and Scopes`

You can also create a keybinding for it.

``` bash
{
  "key": "cmd+alt+shift+i",
  "command": "editor.action.inspectTMScopes"
}
```
