---
title:  "Installing Python Modules into the Correct Python Version on a Mac"
date:   2017-12-01 12:00:00 +1000
categories: python
tags: [mac, feedparser]
---
I'm doing some testing of the Feedparser Python module. The install instructions seem simple enough.

```terminal
sudo pip install feedparser
```

However, on trying to import the module into a Python 3.6 console I received an error message, “ModuleNotFoundError: No module named ‘feedparser'”.

```terminal
Pauls-MacBook-Pro:~ paulcunningham$ python3.6
Python 3.6.1 (v3.6.1:69c0db5050, Mar 21 2017, 01:21:04) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import feedparser
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'feedparser'
```

I ran the installer again, and the error message this time indicated that the module was installed for Python 2.7, which is not the version I'm tying to use.

```
Pauls-MacBook-Pro:tst paulcunningham$ sudo pip install feedparser
Password:

Requirement already satisfied: feedparser in /Library/Python/2.7/site-packages
```

After some reading of the [Python docs for installing modules](https://docs.python.org/3/installing/index.html), I learned that it's necessary to run the version of Pip for the Python version you're installing the new module for.

```
python3.6 -m pip install feedparser
```

Feedparser is now available for me to use in Python 3.6.

```
Pauls-MacBook-Pro:tst paulcunningham$ python3.6
Python 3.6.1 (v3.6.1:69c0db5050, Mar 21 2017, 01:21:04) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import feedparser
>>> 
```