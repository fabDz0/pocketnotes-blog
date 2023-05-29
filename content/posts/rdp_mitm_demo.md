---
title: "RDP MitM Demo"
date: 2019-05-21T07:55:37+05:30
draft: false
tags: ["how-to", "rdp", "mitm"]
---
### Pre-Requisites

We need three virtual machines to recreate this attack. I used virtual box to setup these virtual machines. All three machines must be on same subnet.<!--more-->

All three machines are in “*Nat in network*” mode.\
Attacker machine can be of any OS. Victim should be of Windows OS(any windows OS will do good for demo)

* Virtual Machine 01 – Kali – 10.0.2.9
* Virtual Machine 02 – Windows 7 – 10.0.2.10
* Virtual Machine 03 – Windows 10 – 10.0.2.11

### Tool Setup
* Download “seth” from GitHub. [Link to Download](https://github.com/SySS-Research/Seth)
* Go to the folder where tool is downloaded.

### Follow steps in video below
[Demo](https://www.youtube.com/watch?v=ZAYbd7e3AZo)

{{< youtube ZAYbd7e3AZo >}}
