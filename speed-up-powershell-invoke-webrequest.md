---
id: 20200814151844
uuid: ac8a218b-2e69-4ce7-a2c2-00d0d2b80cb8
title: Speed Up PowerShell Invoke-WebRequest
date: 2020-08-14 15:18:44
modified: 
types: tech-note
categories: powershell
pinned: false
tags: [windows, powershell]
private: false
draft: false
---

If you don't disable the progress bar when downloading a file using `Invoke-WebRequest` on Windows PowerShell, it will take significantly longer to download. 

To disable it run the below:

```powershell
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest <params>
```

The `$ProgressPreference` variable will only last as long as the current PowerShell session.
