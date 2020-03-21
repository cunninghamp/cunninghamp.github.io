---
title: "Retrieving the Public IP Address of a Computer with Powershell"
categories:
  - PowerShell
tags:
  - whatismyip
  - dyndns
  - 'html parsing'
excerpt: "Get-PublicIPAddress.ps1 is a PowerShell script for retrieving the public IP address of a local or remote computer."
---

While I was writing a chapter for an upcoming book I needed to demonstrate how to retrieve the public IP address of a computer. Obviously there are websites like [What is My IP](https://www.whatismyip.com/) that will tell you the answer, but what if you wanted to know the public IP address of a remote computer? For example, a server in a remote datacenter that does not have Internet Explorer installed, or that you don't want to have to RDP to and open a web browser to connect to What is My IP.

***Update: after I published this [Ravikanth Chaganti](http://www.ravichaganti.com/blog/) helpfully pointed out an API that can be queried for the same result, but much cleaner and simpler to work with. I've [updated the script on Github](https://github.com/cunninghamp/PowerShell-General/tree/master/Get-PublicIPAddress) and will leave the old version (and the rest of this blog post) intact as an example of HTML parsing.***

With PowerShell it is reasonably simple to query a web page and parse the HTML, so pulling out the IP address from such as web page is possible. I chose to use [DynDNS](http://checkip.dyndns.com/) for this task, as it has much cleaner output that I found easier to work with.

```
PS C:\> $url = "http://checkip.dyndns.com"

PS C:\> $webrequest = Invoke-WebRequest $url

PS C:\> $webrequest.ParsedHtml.body.innerHtml
Current IP Address: 203.206.161.219
```

That works just fine, but I wanted to write a script that would return only the IP address. Furthermore, I wanted the script to work for the local computer or a remote computer.

That's when I ran into a little problem. The result that was returned by Invoke-WebRequest for the local computer was slighly different to that returned by a remote computer. The remote computer result would not parse using the same method. I spent a few minutes searching for a solution before I decided to just switch to a workaround that would avoid the issue for now.

```powershell
#We just want the plain HTML from the web request.
$RawHtml = $webrequest.Content

#I'm using this method to parse the result because I was seeing two different results
#come back for local computers (ParsedHtml : mshtml.HTMLDocumentClass) vs
#remote computers (ParsedHtml : System.__ComObject).

$HtmlObject = New-Object -ComObject "HTMLfile"
$HtmlObject.IHTMLDocument2_Write($RawHtml)

$result = $HtmlObject.body.innerHTML
```

Now **$result** gives me the same output as the earlier example, so the last task was to clean it up to only return the IP address.

```powershell
#Tidy the result so just the IP is left
$ip = $result.Split(":")[1].Trim()
```

The resulting script, Get-PublicIPAddress.ps1, is available on [Github](https://github.com/cunninghamp/PowerShell-General/tree/master/Get-PublicIPAddress). It has a single, optional parameter that is used to specify a remote computer to run against. You'll receive a credential prompt in that case. I'm using Write-Host for simple output but you can easily use the $ip variable in any way you need to if you put this script code to greater use in your own projects.

```
PS C:\Scripts> .\Get-PublicIPAddress.ps1 -Computer 192.168.0.11
The public IP address of computer 192.168.0.11 is 203.206.161.219

PS C:\Scripts> .\Get-PublicIPAddress.ps1
The public IP address of the local computer is 203.206.161.219
```