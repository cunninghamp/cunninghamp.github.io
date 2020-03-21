---
title: "Get-ADInfo.Ps1 – Powershell Script for Collecting Active Directory Information"
categories:
  - PowerShell
tags:
  - scripts
  - 'active directory'
excerpt: "Get-ADInfo.ps1 is a PowerShell script for collecting details about an Active Directory environment."
---

[Sometimes I just need a way to quickly find out some information about an Active Directory environment](https://practical365.com/exchange-server/powershell-script-active-directory-system-requirements-fsmo-roles-functional-level-global-catalog/). In particular, details such as the functional mode of the forest as well as each domain, the FSMO role holders, and the global catalog servers in each AD site.

I've written a PowerShell script that pulls this information for me and outputs it to the console as well as to a HTML file. The HTML file is useful for when the info needs to be bundled into some documentation or reports about the environment.

You can find the script on Github – [Get-ADInfo.ps1](https://github.com/cunninghamp/Powershell-Exchange/tree/master/ADInfo)

The script requires the Active Directory module for PowerShell, so you can run it on a DC or anywhere the RSAT tools are installed.

```
[PS] C:\Scripts>.\Get-ADInfo.ps1
*** Forest: exchangeserverpro.net ***

Forest Mode: Windows2003Forest
Schema Master: S1DC1.exchangeserverpro.net
Domain Naming Master: S1DC1.exchangeserverpro.net
UPN Suffixes: esp.local

*** Domain: exchangeserverpro.net ***

NetBIOS Name: ESPNET
Domain Mode: Windows2003Domain
PDC Emulator: S1DC1.exchangeserverpro.net
Infrastructure Master: S1DC1.exchangeserverpro.net
RID Master: S1DC1.exchangeserverpro.net

*** Global Catalogs by Site/OS ***

Site, OS                                       Count
--------                                       -----
DataCenter1, Windows Server 2012 R2 Datacenter     1
```