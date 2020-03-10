---
title: "/r/dailyprogrammer Challenge #383 [Easy] Necklace Matching in Python"
excerpt: "My Python solution for /r/dailyprogrammer challenge #383 [Easy]."
categories:
  - Python
tags:
  - reddit
  - dailyprogrammer
---

Details of the challenge [here](https://www.reddit.com/r/dailyprogrammer/comments/ffxabb/20200309_challenge_383_easy_necklace_matching/).

This challenge requires some string manipulation, so my first step was to remind myself of some Python string basics.

```python
>>> string = "thisisastring"
>>> print(string)
thisisastring
```

Strings are zero-indexed, so to reference the first character in the string in Python is as follows:

```python
>>> string[0]
't'
```

To reference all but the first character in the string:

```python
>>> string[1:]
'hisisastring'
```

To "slice" the string so that the first character moves to the end:

```python
>>> newstring = string[1:] + string[0]
>>> print(newstring)
hisisastringt'
```

A simple function that compares two inputs and returns the result (match) of True/False.

```
def same_necklace(first, second):
    match = first == second
    print(match)


same_necklace("nicole", "nicole")
```

But we also need to check if the two inputs are the same characters reordered, per the challenge. A for loop that moves the first character of the string to the end will achieve that.

```
def same_necklace(first, second):
    match = first == second

    if not match:
        for i in range(len(second)):
            second = second[1:] + second[0]
            print('Reordered string is: ' + second)
            if first == second:
                match = True
                break
    print(match)

same_necklace("nicole", "icolen")
```

The above code produces this output:

```
Reordered string is: coleni
Reordered string is: olenic
Reordered string is: lenico
Reordered string is: enicol
Reordered string is: nicole
True
```

So the loop checked each possible reordered string of characters, and found that one of them was a match (True). The loop breaks as soon as a match is found, to avoid testing further combinations of reordered characters.


My final solution with comments:

```python
def same_necklace(first, second):
    # Check for an immediate match
    match = first == second

    # If no match, time to start reordering characters and comparing
    if not match:
        # The loop will run the same number of times as the number of characters in the string
        for i in range(len(second)):
            # Strings are zero-indexed, so to get the string from the second character to the last character
            # would be:
            # print(second[1:])
            #
            # And to get the first character:
            # print(second[0])
            #
            # Loop for the same number of times as the length of the second string which is reordered
            # by one character each time
            second = second[1:] + second[0]
            if first == second:
                match = True
                break
    print(match)
```

The code above produces the expected output when tested with the examples in the challenge.

```python
same_necklace("nicole", "icolen") => true
same_necklace("nicole", "lenico") => true
same_necklace("nicole", "coneli") => false
same_necklace("aabaaaaabaab", "aabaabaabaaa") => true
same_necklace("abc", "cba") => false
same_necklace("xxyyy", "xxxyy") => false
same_necklace("xyxxz", "xxyxz") => false
same_necklace("x", "x") => true
same_necklace("x", "xx") => false
same_necklace("x", "") => false
same_necklace("", "") => true
```