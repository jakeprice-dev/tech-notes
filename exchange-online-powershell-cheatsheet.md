---
id: 20201125172111
uuid: 631ce89e-14ab-46e3-aa4e-4a86aca24fc3
title: Exchange Online PowerShell
date: 2020-11-25 17:21:11
modified: 
types: tech-note
categories: powershell
tags: [exchange, azure, office, powershell]
private: false
draft: false
---

## Connect to Exchange Online PowerShell

```powershell
# Install module:
Install-Module -Name ExchangeOnlineManagement -RequiredVersion 2.0.3 -Confirm:$false

# Load the EXO V2 module:
Import-Module ExchangeOnlineManagement

# Connect to Exchange Online PowerShell:
Connect-ExchangeOnline -UserPrincipalName jake.price@example.com -ShowProgress $true
```

## Helpful Commands:

```powershell
# Remove user's photo:
Remove-UserPhoto -Identity <user>@example.com -Confirm:$false
```
