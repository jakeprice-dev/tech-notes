---
id: 20201209152358
uuid: c9c3add0-7a28-43e3-9cff-622b4ce0c883
title: Connect to Azure AD in PowerShell
date: 2020-12-09 15:23:58
modified: 
types: tech-note
categories: azure
tags: [azure-ad, powershell]
private: false
draft: false
---

{{<admonition tip>}}
Make sure you have run the `Install-Module AzureAD` command.
{{</admonition>}}

In PowerShell type `Connect-AzureAD`, and type in your credentials in the login box that pops-up.

```powershell
$ Connect-AzureAD

Account                           Environment TenantId                             TenantDomain           AccountType
-------                           ----------- --------                             ------------           -----------
jake.price@example.com AzureCloud  e1d6ec7d-132c-49c2-881f-8d398e59a311 example.com User
```
