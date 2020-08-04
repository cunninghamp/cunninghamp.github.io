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

According to the [FiscalYear module docs](https://pypi.org/project/fiscalyear/):

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

The end date is the start date *plus* 31 days. Note that if a month has fewer than 31 days, selecting the 31st day will still land on the last day of the month, such as the 30th, 29th, or 28th. I have seen sample code that always adds 32 days, I guess just to be sure?

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

>>> for m in range(0, 12):
...     next_month_start = d + relativedelta.relativedelta(months=m, day=1)
...     print(f"{next_month_start.strftime('%b %d, %Y')}")
...
Jul 01, 2020
Aug 01, 2020
Sep 01, 2020
Oct 01, 2020
Nov 01, 2020
Dec 01, 2020
Jan 01, 2021
Feb 01, 2021
Mar 01, 2021
Apr 01, 2021
May 01, 2021
Jun 01, 2021
```

Similarly, we can calculate the first *and* last day of each month in a fiscal year.

```python
current_fiscal_year = FiscalYear.current()

d = datetime.datetime(current_fiscal_year.start.year, current_fiscal_year.start.month, current_fiscal_year.start.day)

for m in range(0, 12):
    next_month_start = d + relativedelta.relativedelta(months=m, day=1)
    next_month_end = d + relativedelta.relativedelta(months=m, day=31)
    print(f"{next_month_start.strftime('%b %d, %Y')} to {next_month_end.strftime('%b %d, %Y')}")

Jul 01, 2020 to Jul 31, 2020
Aug 01, 2020 to Aug 31, 2020
Sep 01, 2020 to Sep 30, 2020
Oct 01, 2020 to Oct 31, 2020
Nov 01, 2020 to Nov 30, 2020
Dec 01, 2020 to Dec 31, 2020
Jan 01, 2021 to Jan 31, 2021
Feb 01, 2021 to Feb 28, 2021
Mar 01, 2021 to Mar 31, 2021
Apr 01, 2021 to Apr 30, 2021
May 01, 2021 to May 31, 2021
Jun 01, 2021 to Jun 30, 2021
```

Finally, if I'm looking at multiple fiscal years, I can calculate the start and finish dates of each month in recent fiscal years.

```python
import fiscalyear
from fiscalyear import *
import datetime
from dateutil import relativedelta

fiscalyear.setup_fiscal_calendar(start_month=7)

first_fiscal_year = 2018
current_fiscal_year = FiscalDate.today().fiscal_year
number_of_loops = current_fiscal_year - first_fiscal_year + 1

for x in range(0, number_of_loops):
    fy = FiscalYear(first_fiscal_year + x)
    fystart = fy.start.strftime("%b %d %Y")
    fyend = fy.end.strftime("%b %d %Y")
    print(f"The fiscal year {fy.fiscal_year} runs from {fystart} to {fyend}")

    d = datetime.datetime(fy.start.year, fy.start.month,
                          fy.start.day)

    for m in range(0, 12):
        next_month_start = d + relativedelta.relativedelta(months=m, day=1)
        next_month_end = d + relativedelta.relativedelta(months=m, day=31)
        print(f"{next_month_start.strftime('%b %d, %Y')} to {next_month_end.strftime('%b %d, %Y')}")
```

The code above will output the following results.

```
The fiscal year 2018 runs from Jul 01 2017 to Jun 30 2018
Jul 01, 2017 to Jul 31, 2017
Aug 01, 2017 to Aug 31, 2017
Sep 01, 2017 to Sep 30, 2017
Oct 01, 2017 to Oct 31, 2017
Nov 01, 2017 to Nov 30, 2017
Dec 01, 2017 to Dec 31, 2017
Jan 01, 2018 to Jan 31, 2018
Feb 01, 2018 to Feb 28, 2018
Mar 01, 2018 to Mar 31, 2018
Apr 01, 2018 to Apr 30, 2018
May 01, 2018 to May 31, 2018
Jun 01, 2018 to Jun 30, 2018
The fiscal year 2019 runs from Jul 01 2018 to Jun 30 2019
Jul 01, 2018 to Jul 31, 2018
Aug 01, 2018 to Aug 31, 2018
Sep 01, 2018 to Sep 30, 2018
Oct 01, 2018 to Oct 31, 2018
Nov 01, 2018 to Nov 30, 2018
Dec 01, 2018 to Dec 31, 2018
Jan 01, 2019 to Jan 31, 2019
Feb 01, 2019 to Feb 28, 2019
Mar 01, 2019 to Mar 31, 2019
Apr 01, 2019 to Apr 30, 2019
May 01, 2019 to May 31, 2019
Jun 01, 2019 to Jun 30, 2019
The fiscal year 2020 runs from Jul 01 2019 to Jun 30 2020
Jul 01, 2019 to Jul 31, 2019
Aug 01, 2019 to Aug 31, 2019
Sep 01, 2019 to Sep 30, 2019
Oct 01, 2019 to Oct 31, 2019
Nov 01, 2019 to Nov 30, 2019
Dec 01, 2019 to Dec 31, 2019
Jan 01, 2020 to Jan 31, 2020
Feb 01, 2020 to Feb 29, 2020
Mar 01, 2020 to Mar 31, 2020
Apr 01, 2020 to Apr 30, 2020
May 01, 2020 to May 31, 2020
Jun 01, 2020 to Jun 30, 2020
The fiscal year 2021 runs from Jul 01 2020 to Jun 30 2021
Jul 01, 2020 to Jul 31, 2020
Aug 01, 2020 to Aug 31, 2020
Sep 01, 2020 to Sep 30, 2020
Oct 01, 2020 to Oct 31, 2020
Nov 01, 2020 to Nov 30, 2020
Dec 01, 2020 to Dec 31, 2020
Jan 01, 2021 to Jan 31, 2021
Feb 01, 2021 to Feb 28, 2021
Mar 01, 2021 to Mar 31, 2021
Apr 01, 2021 to Apr 30, 2021
May 01, 2021 to May 31, 2021
Jun 01, 2021 to Jun 30, 2021

```