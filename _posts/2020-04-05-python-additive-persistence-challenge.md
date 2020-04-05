---
title: "/r/dailyprogrammer Challenge #374 [Easy] Additive Persistence"
categories:
  - Python
tags:
  - recursion
  - dailyprogrammer
  - reddit
excerpt: "My solution for /r/dailyprogammer challenge #374 [Easy]."
---

Details of this challenge are [here](https://www.reddit.com/r/dailyprogrammer/comments/akv6z4/20190128_challenge_374_easy_additive_persistence/).

> Inspired by this tweet, today's challenge is to calculate the additive persistence of a number, defined as how many loops you have to do summing its digits until you get a single digit number. Take an integer N:
> 
> 1. Add its digits
> 2. Repeat until the result has 1 digit
> 
> The total number of iterations is the additive persistence of N.
> 
> Your challenge today is to implement a function that calculates the additive persistence of a number.

Right away I knew that this could be achieved with a recursive function, and a variable tracking the number of "loops" through the function.

However, I wasn't quite sure how to break down a given number, such as "1234", into individual digits that can then be added together. With a little Googling I found the Python 3 [map function](https://docs.python.org/3/library/functions.html#map).

Example:

```python
number = int(input("Enter a number: "))

digits = map(int, (str(number)))
```

The above code returns an object, **digits**, containing the individual digits of the input number. It is then possible to iterate through the object and do something with those individual digits.

```
>>> number = int(input("Enter a number: "))
Enter a number: 1234
>>> digits = map(int, (str(number)))
>>> for i in digits:
...     print(i)
... 
1
2
3
4
```

I built a function to:

1. Create the object using the map function
2. Add the numbers together, storing the answer as **sumofnumbers**
3. Check if **sumofnumbers** is greater than 9 (the goal is to get an answer that is a single digits, so an answer of 10 or more means we're not finished yet)
4. Run the function as many times as required to get an answer of 9 or less, incrementing the loop counter each time

So, if the original input number is less than 10, the answer is 0, because no loops through the function were necessary.

If the original input number is 10 or greater, the function runs as many times as required to get a single-digit answer.

Here's my full solution:

```python
# Ask the user for a number
number = int(input("Enter a number: "))

loopcount = 0


def calculate_additive_persistence(inputnumber):

    # Using global variable so that it doesn't reset to 0 on each recursive loop
    global loopcount
    sumofnumbers = 0

    # Creates a list object containing the individual digits of the input number
    digits = map(int, (str(inputnumber)))

    # Calculate the sum of the individual digits
    for i in digits:
        sumofnumbers = sumofnumbers + i
    print(f"Sum of numbers is: {sumofnumbers}")

    # Increment the loop count
    loopcount += 1

    # If the sum of the individual digits is greater than 9 (i.e. more than 1 digit
    # long), do a recursive loop of the function
    if sumofnumbers > 9:
        calculate_additive_persistence(sumofnumbers)

    # Finally, return the number of times the function ran (looped)
    return loopcount


# If the input number is less than 10 the answer is 0, no loops required
if number < 10:
    answer = 0
else:
    answer = calculate_additive_persistence(number)

print(f"Loops required: {answer}")
```

Example run, in which 3 loops were necessary to achieve a single-digit answer of 3:

```
Enter a number: 199
Sum of numbers is: 19
Sum of numbers is: 10
Sum of numbers is: 1
Loops required: 3
```
