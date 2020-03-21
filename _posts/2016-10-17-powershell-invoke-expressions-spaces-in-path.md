---
title: "Using Invoke-Expression in Powershell with Spaces in Paths"
categories:
  - PowerShell
tags:
  - Invoke-Expression
excerpt: "How to solve errors thrown when using PowerShell’s Invoke-Expression cmdlet for a path that contains spaces."
---

One of the early bugs found in [Exchange Analyzer](https://exchangeanalyzer.com) was an error being thrown when the final HTML report was displayed. I had been developing the script in a folder path of *X:\Scripts\ExchangeAnalyzer*.  In other words, a path with no spaces. Some of the early users then tried to run the script from a path such as *X:\Scripts\Exchange Analyzer* and reported errors.

Here's an example of what went wrong. Let's say we've got an HTML file in C:\Scripts, and it's called “Test Document.html”. If I use [Invoke-Expression](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7) to launch that HTML file, an error will be thrown:

```
PS C:\Scripts> Invoke-Expression '.\Test Document.html'

.\Test : The term '.\Test' is not recognized as the name of a cmdlet, function, script file,
or operable program. Check the spelling of the name, or if a path was included, verify that
the path is correct and try again.
```

As it turns out, Invoke-Expression doesn't handle spaces in paths like that. But there's a couple of easy fixes that you can use.

One fix is to use the following command instead:

```
PS C:\Scripts> Invoke-Expression "& '.\Test Document.html'"
```

That will also work if the path to the document is stored in a variable.

```
PS C:\Scripts> $document = "C:\Scripts\Test Document.html"

PS C:\Scripts> Invoke-Expression "& '$document'"
```

The other option is to escape the spaces in the path, for example:

```
PS C:\Scripts> $document = "C:\Scripts\Test Document.html"

PS C:\Scripts> $document = $document -replace ' ','` '

PS C:\Scripts> Invoke-Expression $document
```

The first option is probably better, as it doesn't require you to modify the existing variable or set a new variable for the path with escaped spaces. But either option should solve the problem of using Invoke-Expression with spaces in the path.