---
title: "Why PowerShell?"
categories:
  - PowerShell
tags:
  - thoughts
  - learning
  - coding
excerpt: "PowerShell has been around for many years now, but it’s still a source of confusion, anger, and frustration for a lot of IT professionals."
---

PowerShell has been around for many years now, but it's still a source of confusion, anger, and frustration for a lot of IT professionals. To this day I still receive frequent comments and emails from people who are either:

- Angry that a GUI management tool was replaced by PowerShell, or is less functional than PowerShell
- Unsure which PowerShell commands to run for a task, and how to run them
- Confused as to how a script or series of commands works, and unable to work out how to modify it for their situation

All of that is okay. We're all beginners at something (lots of things, actually), and PowerShell is just another one of those things that has a learning process that you need to go through.

I will say though, if you're in that first group of people who are angry about the dominance of PowerShell in the lives of Microsoft IT pros today, this article is not going to try hard to convince you to feel any better. Sure, if we happen to meet at a conference or event, I'm happy to have a beer with you and hear your story. But if you're going to read any further, it should be because you've decided that no matter how you happen to feel about it, PowerShell is here to stay and it's important for IT professionals to understand it.

## A Little History

A long time ago, I was a desktop administrator. My job involved responding to tickets that were escalated from the help desk. Often this meant taking a long walk to the end user's desk to fix a problem, because the help desk had been unable to use our remote control software to help them (it was constantly breaking).

I didn't like taking those longs walks, particularly when it involved leaving the building to go across town in the middle of a hot Summer's day. So I started looking for ways that I could remotely fix the most common problems with our desktop fleet (starting with that pesky remote control software). Using batch files and tools such as PSExec, I pretty quickly assembled a tool kit of useful scripts that saved me a lot trips. Maybe less walking wasn't the best thing for my health at the time, but it certainly was good for my efficiency on the job.

Batch files can be surprisingly powerful, but I soon discovered VBScript and started writing more of my scripts in that language. I got reasonably proficient after a while, and our team tool kit once again expanded with useful scripts that improved our productivity. Many of the scripts were even provided to the help desk, which meant they needed to be pretty resilient to errors and simple to use.

In 2002 I heard about this thing called Monad. I found the [Monad manifesto](http://www.jsnover.com/blog/2011/10/01/monad-manifesto/), written by Microsoft's Jeffrey Snover. It seemed interesting, but we were working so hard at the time I didn't give it more than a cursory examination before continuing on with my work in VBScript.

Fast forward a few more years, and after a string of roles in desktop and server support, my career had landed me in a role with a small consulting firm where I was tagged as “the Exchange guy”. This was shortly after the release of Exchange 2007, which threw a big new challenge at IT pros with its server roles architecture and use of PowerShell for management.

I got through my first few Exchange 2007 projects with a lot of “click, click, Next, Next” configuration, which was pretty slow going. But I noticed that Exchange 2007 would helpfully display the exact PowerShell syntax for the task you were about to perform, and let you copy it to your clipboard and save it for later. Projects started speeding up as I took more and more of the common deployment tasks, copied the PowerShell syntax, and started running the same steps as PowerShell scripts.

After a few more years of mostly working with Exchange, a new job opened up with a very large, multi-national organization. In my home town such opportunities don't come along very often, so I took it. At first my role was simply moving mailboxes from various remote sites to new servers in a centralized datacenter. I pretty quickly scripted the migrations, to the point where I really spent most of my day analyzing data in Excel, working with the change control teams, and helping with the communications sent to each batch of users to be migrated that evening. At the end of the day I dropped a few CSV files in folders and went home, with the heavy lifting all handled by scripts and scheduled tasks overnight. In the morning I would check the results, send a report to the project manager, and start the whole process again.

That might sound a bit dull, but being there in that organization started opening up opportunities to help out with operational support as well. By the time the migration project finished, I had joined the team that managed the Exchange infrastructure for the Asia-Pacific region. Suddenly I was supporting far more users and servers than I ever had before, and I realised there was no way I could keep up without the help of scripting and automation. And that meant a whole lot of time writing PowerShell. Automation became critical to our team's ability to perform our duties, as our head count shrank but our responsibilities grew. Doing “more with less” was the reality that we were dealing with.

In fact, it was in that job that the earliest versions of [Get-MailboxReport.ps1](https://github.com/cunninghamp/Get-MailboxReport.ps1) and [Test-ExchangeServerHealth.ps1](https://github.com/cunninghamp/Test-ExchangeServerHealth.ps1) were born, scripts that have been downloaded tens of thousands of times since. I literally used those scripts every day, and have continued to develop them and use them after I left that company.

## Why Learn PowerShell?

At this point you might be wondering how my story applies to you. Perhaps you are thinking that since you don't work on multiple customer projects, or don't deal with hundreds or thousands of servers, that PowerShell has no real benefit to you. I don't agree with that, because the same tasks that can be performed more efficiently in a 100 user environment can also be performed in a 10,000 user environment.

Tasks such as:

- New user provisioning
- Daily, or quick ad-hoc health checks
- Monthly reports
- Inventory collection
- Copying files from one location to another
- Archiving departed user data
- Creating new virtual machines

If you're going to do a task more than once, you should try to automate it with PowerShell.

Really? Even all those 10 second jobs that aren't made any faster by scripting? Yes! For one thing, it's good practice for writing PowerShell code. The more you write, the better you'll get at it. And secondly, automation of tasks improves accuracy.

What's more, the PowerShell you learn on one task will benefit you in completely different tasks later. Just because I learned my first PowerShell skills working with Exchange, didn't mean I had to re-learn PowerShell again to work with Active Directory, DNS, Hyper-V, or anything else Microsoft releases that is PowerShell-driven (which is pretty much everything these days). PowerShell is PowerShell, and with knowledge of a few cmdlets like Get-Help and Get-Member, PowerShell becomes a self-teaching language.

And, importantly, the PowerShell skills you learn at your current job could help to land you the next (better?) job.