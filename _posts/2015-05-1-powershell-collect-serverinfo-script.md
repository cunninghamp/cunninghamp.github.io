---
title: "Collect-ServerInfo.Ps1 – a Powershell Script for Windows Server Inventory"
categories:
  - PowerShell
tags:
  - scripts
  - 'windows server'
  - inventory
excerpt: "This PowerShell script collects inventory information about Windows Servers and outputs to a HTML file."
---

Many of the customer projects I work on involve collecting an inventory of basic information about the Windows Servers in the environment, such as CPU/memory specs, OS versions, volume sizes, and so on.

To make this inventory process less time consuming I began using PowerShell scripts to collect the information I was interested. Over time these scripts got less messy and more useful, so now I want to share my current script.

This PowerShell script, **Collect-ServerInfo.ps1**, will collect information from Windows Servers that includes:

- Computer system information (name, make, model, processor, memory, domain)
- Operating system information (OS name, version, install date)
- Memory information (capacity, modules, speed)
- Pagefile information (size, location)
- BIOS information (manufacturer, version, release date)
- Logical disk information
- Volume information
- Network interface information
- Software information

The information is output to a HTML file per server.

![](/assets/images/collect-serverinfo-report-example.png)

## Usage Examples

Collect information about a single server named SERVER1.

```
.\Collect-ServerInfo.ps1 SERVER1
```

Collect information about multiple servers.

```
"SERVER1","SERVER2","SERVER3" | .\Collect-ServerInfo.ps1
```

Collects information about all servers in Active Directory.

```
Get-ADComputer -Filter {OperatingSystem -Like "Windows Server*"} | %{.\Collect-ServerInfo.ps1 $_.DNSHostName}
```

## Download

Collect-ServerInfo.ps1 can be downloaded from [Github](https://github.com/cunninghamp/Collect-ServerInfo).