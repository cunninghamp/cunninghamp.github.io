---
title: "ModuleNotFoundError: No module named 'win32com' on Windows 10"
categories:
  - Python
tags:
  - win32com
  - pypiwin32
  - pip
  - windows10
excerpt: "Solution for error message 'No module named win32com' on a Windows 10 computer when running Python scripts."
---

I recently updated my Windows 10 computer to Python 3.8. Either as a result of this, or some other issue, one of my Python scripts began failing.

The script imports `win32com.client` to run Microsoft Excel and refresh some data from a database.

```python
import win32com.client
```

However it begain throwing an error.

```
Traceback (most recent call last):
  File "RefreshReports.py", line 1, in <module>
    import win32com.client
ModuleNotFoundError: No module named 'win32com'
```

With a little digging around I found that the solution was to reinstall the `pypiwin32` package.

```
C:\>pip install pypiwin32

Collecting pypiwin32
  Using cached pypiwin32-223-py3-none-any.whl (1.7 kB)
Collecting pywin32>=223
  Downloading pywin32-228-cp38-cp38-win_amd64.whl (9.1 MB)
     |████████████████████████████████| 9.1 MB 6.8 MB/s
Installing collected packages: pywin32, pypiwin32
Successfully installed pypiwin32-223 pywin32-228
```

The script now runs successfully.