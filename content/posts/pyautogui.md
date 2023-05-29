---
title: "Pyautogui, a module to automate keyboard and mouse function"
date: 2019-07-07T19:28:31+05:30
draft: false
tags: ["python", "pyautogui","how-to"]
---

[*pyautogui*](https://pyautogui.readthedocs.io/en/latest/install.html) is a python module to automate keyboard and mouse actions. I found this very interesting and creepy too ! :).

Here are few things to 📝 note,
<!--more-->
* Tried installing on python 2.7, was not successful
* Use python 3.x to install [pyautogui](https://pyautogui.readthedocs.io/en/latest/install.html)
   * pip3.7.exe install pyautogui

## Sample Script

Open a application in taskbar and close it using mouse clicks.
--> move cursor on application, then in python3.x console run the following code and
--> Note position of application in taskbar 
--> note position of "close" button

```python
Python 3.7.3 (v3.7.3:ef4ec6ed12, Mar 25 2019, 21:26:53) [MSC v.1916 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import pyautogui
>>> pyautogui.position()
Point(x=396, y=591)
>>>pyautogui.position()
Point(x=1884, y=0)
>>>
```
Simple Python code to open and close application (very basic :) )

```python
import pyautogui
decision = 'OK'
while (decision == "OK"):
	pyautogui.click(x=255, y=1048, clicks=1, button='left', duration=3)
	pyautogui.click(x=1876, y=21, clicks=1, button='left', duration=3)
	decision = pyautogui.confirm ('Should I continue ?')
```




