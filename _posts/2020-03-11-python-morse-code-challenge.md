---
title: "/r/dailyprogrammer Challenge #380 [Easy] Smooshed Morse Code in Python"
excerpt: "My Python solution for /r/dailyprogrammer challenge #380 [Easy]."
categories:
  - Python
tags:
  - reddit
  - dailyprogrammer
---

Details of the challenge [here](https://www.reddit.com/r/dailyprogrammer/comments/cmd1hb/20190805_challenge_380_easy_smooshed_morse_code_1/).

This challenge requires converting words into morse code. The morse code for a-z is provided in the challenge (each "letter" is separated by a space).

```
.- -... -.-. -.. . ..-. --. .... .. .--- -.- .-.. -- -. --- .--. --.- .-. ... - ..- ...- .-- -..- -.-- --..
```

To turn that into a list we can use the split() function.

```python
>>> string = ".- -... -.-. -.. . ..-. --. .... .. .--- -.- .-.. -- -. --- .--. --.- .-. ... - ..- ...- .-- -..- -.-- --.."

>>> morse = list(string.split(" "))
>>> morse
['.-', '-...', '-.-.', '-..', '.', '..-.', '--.', '....', '..', '.---', '-.-', '.-..', '--', '-.', '---', '.--.', '--.-', '.-.', '...', '-', '..-', '...-', '.--', '-..-', '-.--', '--..']
```

The Python 3 [string module](https://docs.python.org/3/library/string.html) allows us to easily generate a list of the letters a-z.

```python
>>> import string
>>> string.ascii_lowercase
'abcdefghijklmnopqrstuvwxyz'

>>> alphabet = list(string.ascii_lowercase)
>>> alphabet
['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
```

Using the two lists above, we can find the corresponding morse code for any letter of the alphabet.

```python
>>> letter = 'a'
>>> print(morse[alphabet.index(letter)])
.-
>>>
>>> letter = 'b'
>>> print(morse[alphabet.index(letter)])
-...
```

So let's apply that to an actual word, such as "help". The word "help" can be turned into a list of single characters.

```python
>>> lettersinword = list("help")
>>> lettersinword
['h', 'e', 'l', 'p']
```

Then we can loop through that list, looking up the morse code for each letter in the word.

```python
>>> for i in lettersinword:
...     print(morse[alphabet.index(i)])

....
.
.-..
.--.
```

The challenge requires us to output the morse code as a single string.

```python
>>> output = ""
>>> for i in lettersinword:
...     output = output + (morse[alphabet.index(i)])

>>> print(output)
......-...--.
```

For my final solution, I've made the loop and concatenation of the output into a function, and added some extra words to make the output clearer.

```python
import string


def smorse(word):
    # Create a variable for the output
    output = ""

    # Create a list from the characters of the input word
    lettersinword = list(word)

    # Loop through list and concatenate the corresponding morse code to the output
    for i in lettersinword:
        output = output + (morse[alphabet.index(i)])

    # I've added this for nicer reading
    output = "The morse code for " + word + " is " + output
    return output


# Create a list of the letters a-z
alphabet = list(string.ascii_lowercase)

# Create a list of the individual characters of morse code provided by the challenge
morsecodes = ".- -... -.-. -.. . ..-. --. .... .. .--- -.- .-.. -- -. --- .--. --.- .-. ... - ..- ...- .-- -..- -.-- --.."
morse = list(morsecodes.split(" "))


print(smorse("sos"))
print(smorse("daily"))
print(smorse("programmer"))
print(smorse("bits"))
print(smorse("three"))
```

With the code above I get the expected output for the challenge (with my extra words as well).

```
The morse code for sos is ...---...
The morse code for daily is -...-...-..-.--
The morse code for programmer is .--..-.-----..-..-----..-.
The morse code for bits is -.....-...
The morse code for three is -.....-...
```