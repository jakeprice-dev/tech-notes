---
id: 20201023220029
uuid: ab9e7a17-2138-4030-87e5-e49fa904aa62
title: Disable Windows 10 Automatic Restarts after Updates 
date: 2020-10-23 22:00:29
modified: 
types: tech-note
categories: windows
link: https://answers.microsoft.com/en-us/windows/forum/all/disable-windows-10-automatic-restart-after-updates/16f1826d-a796-4de8-ac99-1d625420d265?auth=1
tags: [windows-10, updates]
private: false
draft: false
---

> 1. Click on the `Start` button and type `gpedit.msc`, press Enter.
> 2. In the Local Group Policy Editor, go to `Computer Configuration -> > Administrative Templates -> Windows Components -> Windows Update`
> 3. Double-click on `No auto-restart with automatic installations of scheduled > updates`.
> 4. Select `Enabled`, and then click `OK`.
> 5. Close the local group policy editor. 
>
> -- [Source](https://answers.microsoft.com/en-us/windows/forum/all/disable-windows-10-automatic-restart-after-updates/16f1826d-a796-4de8-ac99-1d625420d265?auth=1)
