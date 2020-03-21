---
title: "Install-AWSPowershellModule.Ps1 Is a Powershell Script to Install the Amazon Web Services Tools for Windows Powershell"
categories:
  - PowerShell
tags:
  - modules
  - aws
  - amazon
excerpt: "This PowerShell script can be used to automate the install of the Amazon Web Services Tools for Windows PowerShell."
---

Amazon Web Services provides a [PowerShell module](https://aws.amazon.com/powershell/) that can be installed to allow you to manage your AWS services from PowerShell. Downloading and installing the tools is simple enough, but I prefer automation where possible so that new management stations can be set up quickly.

**Install-AWSPowerShellModule.ps1** is a script that will download the latest MSI package from Amazon Web Services and install it on the local computer.

Example:

```
PS C:\Scripts> .\Install-AWSPowerShellModule.ps1
```

You can download the script from [Github](https://github.com/cunninghamp/InstallAWSPowerShellModule). The script contains a function if you need to re-use it in other scripts or build processes of your own.