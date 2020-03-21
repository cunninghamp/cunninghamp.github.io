---
title: "Dev Log: RSS Feed Email Digest"
categories:
  - Python
tags:
  - projects
  - rss
  - devember
excerpt: "A dev log for my Devember challenge – write an RSS to email script using Python."
---

This year I've been trying to learn a new programming language and improve my development skills. My career background in the Microsoft IT pro space has me reasonably proficient in PowerShell, and my use of WordPress for the last 10 years has given me a little bit of PHP, HTML, and CSS skill as well.

But, I don't consider myself a developer, and want to change that by learning more developer skills. I use some of my spare time to read books, watch some training courses, and tinker with different languages. It seems pretty clear to me that I most enjoy web development, so HTML/CSS/Javascript (and some JS framework yet to be decided) is where I'm heading. There's a lot of extra stuff to learn around that as well, but that's the basic direction I'm going in.

But, I'm also interested in Python. All the hours I've spent with Python so far have been enjoyable. And it seems like a good, platform-agnostic language for general automation and tooling (things I would previously have used PowerShell for).

Today I came across [Devember](https://devember.org/), a “month of coding” challenge where participants spend an hour a day doing some type of coding or programming. There are other similar challenges out there, but Devember just happens to come along at the right time for me to jump in.

I've decided to spent my Devember working on a personal project that has been on my wish list for a while now. I have a PowerShell script that runs at various times throughout the week, parses a list of RSS feeds, and emails me a summary of the new content in those feeds. The timing of the emails depends on the topic, for example, at around 7:30pm when I'm relaxing on the couch after dinner with a coffee, I like to flick through the day's tech news headlines.

You might ask why I don't just use an RSS reader such as [Feedly](https://feedly.com/). I do actually use Feedly, just not for low priority stuff. And I don't like my Feedly backlog to get too long. If I don't have time to read the tech news email that day, I just delete the email. If all those feeds were in Feedly, it would be 50 items added to the backlog.

Since I already have a working PowerShell script for this, why write another one? There's three reasons. The first is that it fills a personal need, which is one of the best reasons to create anything. The second reason is that I want to learn Python, and this project has enough little challenges within it to be a good learning process. The third reason is that I want to move away from PowerShell as my primary weapon for this type of task, so the PowerShell script is now more of a burden than a benefit (I keep a Windows VM running for this script and some others that I still use).

## Devember Dev Log

I'll be keeping this post updated as a dev log throughout December. Hopefully I can make it to the end. Anything else that is worth blogging along the way I will write a separate post for.

- Day 1 – Set up [GitHub repository](https://github.com/cunninghamp/rss-feed-email-digest). Basic Feedparser script written.
- Day 2 – Improved script to read RSS feed URLs from a text file, and process each one using a function that reads the feed and prints some info about each feed item.
- Day 3 – Nothing.
- Day 4 – Clearing some pylint messages. I learned what [docstrings](https://www.python.org/dev/peps/pep-0257/) are.
- Day 5 – Learning about custom classes and objects in Python, which are similar to what I have already learned in PowerShell, but different enough that I am running into errors trying to do things the PowerShell way.
- Day 6 – Learned how to use a list and add my custom objects to the list in Python. I have incorporated these learnings into my script, so that it now parses each feed in the source text file, creates a custom object for each feed item to store the title, URL, and date, then adds that object to a list. Next I will be working on using that list to determine which feed items are new, and which have been seen before.
- Day 7 – Learned how to use the [Pickle module](https://docs.python.org/3/library/pickle.html) to [save and retrieve my list data in files](https://code.tutsplus.com/tutorials/serialization-and-deserialization-of-python-objects-part-1--cms-26183), which will be helpful for persisting data to disk and retrieving it later to compare RSS feeds to determine what is new and what has already been seen.
- Day 8 – Travel day.
- Day 9 – Nothing.
- Day 10 – Started working on adding a command line argument/parameter so that I can specify the name of the file containing the list of RSS feeds to check. The idea is so I can run the script against different input files at different days and times during the week, depending on what topics I want an email digest about that day. Ran into some problems and it seems I need to learn about the proper use of “__main__” to solve this.
- Day 11-16 – Nothing, unwell, various other issues.
- Day 17 – Couple of hours of LPTHW lessons to brush up on some bits I'm not clear on.
- Day 18-31 – Nothing. Enjoying my holidays too much!

I did not compete the script. I learned some new things about Python, which was good. I am going to reflect on my goals some more before I decide how I'm going to handle this unfinished business.