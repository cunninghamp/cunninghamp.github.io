---
title: "Calculating Fiscal Year Dates With Python"
categories:
  - Python
tags:
  - datetime
  - dates
  - fiscalyear
excerpt: "Using Python to calculate fiscal year and month date ranges."
---

A lot of the code that I write involves database queries for calculations based on date ranges. For example, I might need to know the number of new customers each month, or the value of invoices each month. Often I am comparing a month to the same month in the previous fiscal year (what we refer to as a "financial year" here in Australia).

In the beginning, a lot of my database queries involves hard-coded date ranges. Most of the time that works fine. Until we roll over to a new fiscal year, and I need to update the date ranges. Or until I want to compare the same month across multiple years, requiring me to define multiple start/finish dates.

So, I've been looking into how to avoid the use of hard-coded, static date ranges.

Python's ```datetime``` module is where I started.

```python
>>> import datetime

>>> todays_date = datetime.date.today()

>>> todays_date
datetime.date(2020, 8, 3)

>>> print(todays_date)
2020-08-03
```

I am mostly interested in date ranges for my scenarios. Any time-based database queries will always begin at 00:00:00 and end at 23:59:59. However, if I was interested in times, I could use ```datetime``` objects instead of ```date``` objects.

```python
>>> todays_date_and_time = datetime.datetime.today()
>>>
>>> print(todays_date_and_time)
2020-08-03 16:09:27.814893
```

The ```datetime``` module doesn't solve my immediate need to calculate fiscal year date ranges. For that, I discovered the ```FiscalYear``` package.

```
C:\>pip install fiscalyear
```

Here in Australia, the fiscal year runs from 1st July to 30th June. For example, the 2020 fiscal year ran from 1st July 2019 to 30th June 2020. So, the FiscalYear module needs to be configured to match that start month.

```python
>>> import fiscalyear
>>> fiscalyear.setup_fiscal_calendar(start_month=7)
```

We also use a different date format than some countries, preferring ```day/month/year```. 1st July 2019 is 01/07/2020, or 1/7/20, and so on.

Let's start by finding out the current fiscal year. Based on today's date, we're in the 2021 fiscal year.

```python
>>> from fiscalyear import *
>>> FiscalYear.current()
FiscalYear(2021)
```

We can also learn the start and end dates and times for the current fiscal year.

```python
>>> FiscalYear.current().start
FiscalDateTime(2020, 7, 1, 0, 0)

>>> FiscalYear.current().end
FiscalDateTime(2021, 6, 30, 23, 59, 59)
```

According to the FiscalYear module docs:

> The start and end of each quarter are stored as instances of the ```FiscalDateTime``` class. This class provides all of the same features as the ```datetime``` class, with the addition of the ability to query the fiscal year and quarter.

The fiscal quarter capability is interesting, though I won't often need to use it.

```python
>>> FiscalQuarter.current()
FiscalQuarter(2021, 1)

>>> FiscalQuarter.current().start
FiscalDateTime(2020, 7, 1, 0, 0)

>>> FiscalQuarter.current().end
FiscalDateTime(2020, 9, 30, 23, 59, 59)
```

But, the similarity with the ```datetime``` class makes it easy to use FiscalYear calculations in other ```datetime``` calculations. For example, if I want to output the start date of the fiscal year in another format:

```python
>>> fy = FiscalYear.current()

>>> fy.end
FiscalDateTime(2021, 6, 30, 23, 59, 59)

>>> end_of_fy = fy.end.strftime("%b %d %Y %H:%M:%S")

>>> print(f'The end of the current financial year is {end_of_fy}')
The end of the current financial year is Jun 30 2021 23:59:59
```

We can also use ```dateutil``` to calculate relative dates using ```relativedelta```. For example, the start date of the fiscal year is 1/7/2020.

```python
>>> fy.start
FiscalDateTime(2020, 7, 1, 0, 0)

>>> start_date = datetime.date(fy.start.year, fy.start.month, fy.start.day)
>>> start_date
datetime.date(2020, 7, 1)
```

The end date is the start date *plus* 31 days. Note that if a month has fewer than 31 days, adding 31 days will still land on the last day of the month. I have seen sample code that always adds 32 days, I guess just to be sure?

```python
>>> from dateutil import relativedelta

>>> end_date = start_date + relativedelta.relativedelta(day=31)

>>> end_date
datetime.date(2020, 7, 31)
```

Similarly, the first day of the *next* month would be the start date *plus* 1 month.

```python
>>> start_date
datetime.date(2020, 7, 1)

>>> next_month = start_date + relativedelta.relativedelta(months=1)

>>> next_month
datetime.date(2020, 8, 1)
```

For example, after calculating the first day of a fiscalyear, we can then calculate the first day of each month in that fiscal year.

```python
>>> current_fiscal_year = FiscalYear.current()

>>> print(current_fiscal_year.fiscal_year)
2021

>>> d = datetime.datetime(current_fiscal_year.start.year, current_fiscal_year.start.month, current_fiscal_year.start.day)

>>> print(f"The current financial year is {current_fiscal_year.fiscal_year}")
The current financial year is 2021

>>> print(f"It begins on {d.strftime('%b %d, %Y')}")
It begins on Jul 01, 2020

```
