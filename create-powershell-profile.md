---
id: 20200619115742
uuid: 28ba907b-7a74-4634-b0a4-2e825d6c7b43
title: Create a PowerShell Profile
date: 2020-06-19 11:57:42
modified: powershell
types: tech-note
categories: 
pinned: false
tags: [powershell, profile, windows]
private: false
draft: false
---

```powershell
# Check if profile already exists:
Test-Path $profile

# Create a profile if above returns false:
New-Item -path $profile -type file –force

# Profile created at below path, ready for editing:
C:\Users\<username>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```
