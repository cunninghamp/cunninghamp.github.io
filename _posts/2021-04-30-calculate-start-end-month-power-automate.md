---
title: "Calculate the Start and End of the Month in Power Automate"
categories:
  - Power Automate
tags:
  - microsoft-flow
  - power-automate
  - dates
excerpt: "How to calculate the start and end date of a month using Microsoft Power Automate."
---

A lot of my reporting scenarios require reporting of data within a given month, and comparing month to month results. I often use Microsoft Power Automate for reporting and other business process automation. And I want my reports to be as automated as possible, with no static entries such as date ranges coded into them.

So in this blog post I will demonstrate how I calculate start and end dates of months in Power Automate.

## Calculating the Start and End of the Current Month in Power Automate

For the first example, let's look at a flow that needs to know the start and end date of the current month. To make it even more specific, we will calculate the start and end timestamps all the way down to the exact second. This would capture scenarios such as database entries that might added at any time of day, such as an online store that receives orders from all over the world. It also allows for accuracy when converting local timezone timestamps into UTC timestamps, if your data requires you to do so.

To begin with, I create a simple Flow with a manual trigger.

![New Flow](/assets/images/power-automate-start-end-month/power-automate-new-flow.jpg)

I then add an action to retrieve the current time.

![Retrieving the Current Time](/assets/images/power-automate-start-end-month/power-automate-current-time.jpg)

Before going further, let's look at what is returned for the current time. As you can see, the timstamp is returned in UTC time.

![Result of Current Time](/assets/images/power-automate-start-end-month/power-automate-current-time-result.jpg)

`"body": "2021-04-30T04:34:34.6043852Z"`

It is 2:27pm here in Brisbane (GMT+10) right now, but in UTC time it is 4:27am. To allow for timestamps in local and UTC time I will demonstrate how to calculate both.

## Calculating the Start and End of the Current Month in UTC

To calculate the start of the current month we can use the `startOfMonth` function.

`startOfMonth(body('Current_time'))`

I will store the result in a variable named `StartOfCurrentMonthInUTC`.

![Calculating Start of Current Month in UTC](/assets/images/power-automate-start-end-month/power-automate-startofcurrentmonthinutc.jpg)

Now we have a variable storing the timestamp that respresents the start of the current month in UTC timezone, `2021-04-01T00:00:00.0000000Z`.

![Result of Start of Current Month in UTC](/assets/images/power-automate-start-end-month/power-automate-startofcurrentmonthinutc-results.jpg)

Calculating the end of the month is a little different. There is no `endOfMonth` function to do this for us. And different months of the year have different numbers of days, so we can't simply add 30 days. We're also looking for accuracy down to the second, so adding any fixed number of days isn't going to work.

Instead, we can add 1 month to `StartOfCurrentMonthInUTC`, and then **subtract** 1 second, using the `addToTime` and `subtractFromTime` functions in the expression `subtractFromTime(addToTime(variables('StartOfCurrentMonthInUTC'),1,'month'),1,'second')`.

![Calculating End of Current Month in UTC](/assets/images/power-automate-start-end-month/power-automate-endofcurrentmonthinutc.jpg)

I'm storing the results in a variable named `EndOfCurrentMonthInUTC`, and you might also notice I am renaming the actions in my Flow so that I don't end up with a bunch of generic auto-names like **Initialize Variable** and **Initialize Variable 2**.

We're in April right now, so the end of the month is 30th April at 23:59:59, or `2021-04-30T23:59:59.0000000Z`.

![Result of End of Current Month in UTC](/assets/images/power-automate-start-end-month/power-automate-endofcurrentmonthinutc-results.jpg)

## Calculating the Start and End of the Current Month in Local Time

There's a few extra steps involved if you are working with timezones. Let's consider a scenario in which a database is storing timestamps in UTC format (which is quite normal), but I'm interested in records that are relevant to times in my local timezone.

For example, I want to know the database records added between `1/4/2021 00:00:00` and `30/4/2021 23:59:59` local time. In UTC time that would actually be `31/3/2021 14:00:00` and `SOMETHINGELSE`.

Here's how I can use Power Automate to calculate those UTC times. I start the same as before, by retrieving the current time. Then, I convert that to my local timezone.

![Converting the Current Time to Local Time](/assets/images/power-automate-start-end-month/power-automate-current-time.jpg)

Now I have the current time in my local timezone. I can then calculate the start of the current month in my local timezone using `startOfMonth`, and then convert that time back to UTC using `convertTimeZone`, with the following expression. Notice that `convertTimeZone` takes three parameters; the timestamp you are converting, the timezone to convert from, and the timezone to convert to.

`convertTimeZone(startOfMonth(body('Convert_time_zone')),'E. Australia Standard Time','UTC')`

I'm storing the result in a variable named `StartOfCurrentMonthInUTC` as before. If you have a need to know the timestamp for both scenarios (the start of the month in UTC, **and** the start of the month in local time converted to UTC) within the same flow you would simply need to store them in different named variables.

![Calculating Localized Start of Current Month in UTC](/assets/images/power-automate-start-end-month/power-automate-local-startofcurrentmonthinutc.jpg)

Now the variable represents the UTC timestamp for `1/4/2021 00:00:00` local time, which is `2021-03-31T14:00:00.0000000Z`.

![Result of Localized Start of Current Month in UTC](/assets/images/power-automate-start-end-month/power-automate-local-startofcurrentmonthinutc-results.jpg)

As with the previous example, calculating the end of the current month involves adding 1 month then subtracting 1 second from the start timestamp with the expression `subtractFromTime(addToTime(variables('StartOfCurrentMonthInUTC'),1,'month'),1,'second')`. We don't need to repeat the timezone conversion for this, because we're already feeding it a converted time.

![Result of Localized End of Current Month in UTC](/assets/images/power-automate-start-end-month/power-automate-local-endofcurrentmonthinutc-results.jpg)

## Calculating Start and End of Past or Future Months

Once we have the start and end timestamps of the current month it becomes easy to calculate the start and end timestamps for any past or future months as well. In fact, we only need to know the start of the current month to perform these calculations.

For this example I'm just going to use UTC time without any timezone conversions, just to keep things simple.

For example, the start of one month ago is calculated by subtracting one month from `StartOfCurrentMonthInUTC` (or whatever variable you're using to store that value. The expression is `subtractFromTime(variables('StartOfCurrentMonthInUTC'),1,'month')`.

![Calculating Start of One Month Ago in UTC](/assets/images/power-automate-start-end-month/power-automate-startofonemonthagoinutc.jpg)

It's currently April 2021, so the the result for the start of one month ago is March 1st, or specifically, `2021-03-01T00:00:00.0000000Z`.

![Result of Start of One Month Ago in UTC](/assets/images/power-automate-start-end-month/power-automate-startofonemonthagoinutc-results.jpg)

To calculate the first day of a future month, use a similar expression but with the `addToTime` function instead. For example, `addToTime(variables('StartOfCurrentMonthInUTC'),1,'month')`.

The first day of one month in the future, considering this post is written in April 2021, is `2021-05-01T00:00:00.0000000Z`.

You can add or subtract any number of months that you need to, and store it using any variable name you like. Once you know the start of any past or future month, you can simply add 1 month and subtract 1 second to calculate the end of that month.

## Using a Loop to Calculate Start and End of Month Dates in Power Automate

As flows become more complex you may find yourself struggling to maintain a lot of different variables for all the start and end timestamps that you need. It's also cumbersome to duplicate so many variables to multiple flows.

To make things easier you can use a loop to generate the start and end timestamps for the months you need. For example, generate the start and end timestamps for six past months, the current month, and six future months.

I'm going to use a `Do Until` loop for this. I set a variable `MonthToCalculate` that starts at -6, and is incremented by 1 each loop until a max of 6. In other words, calculate start/end timestamps starting at 6 months in the past, all the way up to 6 months in the future.

![Controlling Do Until Loop](/assets/images/power-automate-start-end-month/power-automate-loop-variables.jpg)

The actions in the loop itself are two Compose actions to calculate the start/end timestamps, and then an action to increment the `MonthToCalculate` variable for the next iteration of the loop.

![Loop Actions to Calculate Start and End Dates](/assets/images/power-automate-start-end-month/power-automate-start-end-date-loop-actions.jpg)

The formulas are:

* To calculate the start timestamp, `addToTime(startOfMonth(utcnow())),variables('MonthToCalculate'),'month')`.
* To calculate the end timestamp, `subtractFromTime(addToTime(outputs('Compose_-_CalculateStartTimestamp'),1,'month'),1,'second')`

Note that if you name your Compose actions something else, you'll need to change `outputs('Compose_-_CalculateStartTimestamp')` in the above example to match your action name.

Of course, in a real flow you would then add actions for what you want to do with those timestamps, such as run database queries and then save the results to an Excel Online workbook. But now you have just two actions, the first to set the `MonthToCalculate` variable, and the second to run the `Do Until` loop, when you want to copy/paste them to other flows.