---
title:  "Python 3 Print Function Without Newline"
date:   2017-12-02 12:00:00 +1000
categories: category
tags: [tag1, tag2, tag3]
excerpt: "Using the Python 3 print function without appending a newline to the output."
---

Python's print function automatically adds a newline to the output. When you're printing a single line, the newline isn't noticed.

```
>>> print("A line of text.")
A line of text.
>>>
```

However, if you are reading lines from a text file, the newline is more noticeable. Here is a simple text file.

```
This is line 1.
This is line 2.
This is line 3.
```

Here is a simple Python script to read the text file and print each line.

```
file = open("list.txt", "r")
 
for line in file:
	print(line)
The output looks like this, with the unwanted newlines in between each line.

This is line 1.

This is line 2.

This is line 3.
```

To avoid the unwanted newlines, add an end parameter to the print function.

```
file = open("list.txt", "r")

for line in file:
	print(line, end="")
```

Now the output is cleaner, without the unwanted newlines.

```
Pauls-MacBook-Pro:Scripts paulcunningham$ python3.6 printlist.py 
This is line 1.
This is line 2.
This is line 3.
```