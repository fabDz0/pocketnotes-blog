---
title: "Capture WPA2 Handshake on 5Ghz "
date: 2019-07-08T22:26:15+05:30
draft: false
tags: ["wifi", "wifipineapple", "5ghz", "how-to"]
---

In this blog post [wifi-pineapple](https://shop.hak5.org/products/wifi-pineapple) will be used to capture WPA2 handshake on 5GHz access point. Atheros based chipset is used in Wifi pineapple.
<!--more-->
Currently, there are no modules in wifi pineapple which automate WPA2 handshake capture for 5Ghz range.Manually we can capture 5Ghz handshakes with small changes to settings. 

By default, airmon-ng enables monitor mode on 2.4Ghz channels.\
Change 2.4Ghz channel to a 5Ghz channel. 

Verify if this change is applied. Then run airodump-ng on required 5Ghz channel and capture the handshake.

### Steps
* enable monitor mode (airmon-ng start “card”)
* Change car channel to a 5Ghz channel
(iwconfig “interface” channel “channel number”)
* Run airodump-ng on given channel 
(airodump-ng “interface” –channel “channel number”

### Video link below
WPA Handshake on 5GHz using Wifi Pineapple tetra.
{{< youtube SdzN5ZHsgvo >}}





