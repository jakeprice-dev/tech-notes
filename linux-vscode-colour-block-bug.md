---
id: 20200315122053
uuid: 98d90302-4a7f-4d24-8146-79dc8feb2094
title: Linux and Visual Studio Code Colour Block Bug
date: 2020-03-15 12:20:53
modified: 
types: tech-note
categories: vscode
pinned: false
tags: [linux, vscode, bug]
private: false
draft: false
---

In vscode open the command palette with Ctrl-Shift-P and then "Configure Runtime Arguments". In the `argv.json` file that opens, set `"disable-color-correct-rendering":` to `false`. This seems to fix the weird issues with background colour blocks that I get on Linux.
