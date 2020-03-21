---
title: "New-LabUsers.Ps1 Is a Powershell Script to Create Active Directory Users in a Test Lab"
categories:
  - PowerShell
tags:
  - scripts
  - 'active directory'
  - 'user management'
excerpt: "This PowerShell script can be used to automate creation of Active Directory users with randomly generated passwords in a test lab environment."
---

I build a lot of test and demo Active Directory environments, and one of the steps is creating a bunch of user accounts for testing purposes. I've done this in the past with a few different scripts that others have published, but never had one that worked exactly the way I wanted it to.

One of the problems has been the source of the names. Either the scripts use unrealistic names like “TestUser001”, or they supply a list of realistic looking names that I don't know the origin of. For all I know they could be real names from the address list of some company, which is problematic if I want to use those names for examples in one of my books.

The other problem is usually with passwords. Scripts often come with a generic password that is the same for all of the users that it creates. Worse, some of them display the password in the PowerShell console as the users are created, which is awkward when I'm trying to do live or recorded demos.

Rather than hack existing scripts I decided to start from scratch and make my own.

First, I needed a source of random names. I briefly contemplated typing them out manually, or reaching for our old book of baby names, but then I stumbled across the [List of Random Names](http://listofrandomnames.com/) website. Problem solved. I quickly generated a few lists and merged them into one text file.

Second, I needed a random password generator. After a little searching I found [this excellent PowerShell function](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/03/generating-a-new-password-with-windows-powershell.aspx). With just a few tweaks it was ready to use in my script. For the test accounts I actually want to log on with I'll just reset the password first.

Finally, I started to write the script. My objective with the script was to:

- Check for the OU structure I use in test lab environments, and create it if it does not already exist. I also added a parameter to specify a different top-level OU name when needed.
- Create the users from the random name list, with randomly generated passwords.

I also added population of the Department attribute based on random selection from a list of departments, and also added the City, Country, and a randomly generated 555-xxxx phone number.

I've published the finished script to Github, which you can find here.

- [New-LabUsers.ps1 on Github](https://github.com/cunninghamp/New-LabUsers.ps1)

This script relies on the Active Directory PowerShell module. If you are running it on your test lab domain controller the module should already be present and the script should work. I have tested the script on Windows Server 2012 R2 only at this stage.

All parameters are optional. The script will default to use:

- The input file RandomNameList.txt
- Password length of 16 characters
- Top-level OU of “Company”

Use the script parameters if you need to change those values:

- **InputFileName** – Use this parameter if you need to specify a different text file name containing the list of users. A generated list of names is also provided with the script so that you can see the format required.
- **PasswordLength** – Use this parameter if you need to specify a different password length. By default all the user accounts are created with a randomly generated password that is 16 characters long. You can reset the password for any user that you want to log on with.
- **OU** – Use this parameter if you want to name the top-level OU something different than the default name of “Company”.

Examples:

Uses the RandomNameList.txt file to generate the list of user accounts in an OU called “Company” in Active Directory.

```
.\New-LabUsers.ps1
```

Uses the MyNames.txt file to generate the list of user accounts in an OU called “TestLab” in Active Directory, with 8 character passwords.

```
.\New-LabUsers.ps1 -InputFileName .\MyNames.txt -PasswordLength 8 -OU TestLab
```