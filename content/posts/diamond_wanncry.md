---
title: "Analyze WannaCry Ransomware with Diamond Model"
date: 2020-06-28T08:12:18+05:30
draft: false
tags: ["threatintel", "wannacryRansomware", "diamondModel","notes"]
---

![WannaCry in Diamond Model](/img/diamondModel.jpg)

In this blog post, will explore how wanna cry ransomware event can be mapped with Diamond model.
<!--more-->
Check this [blog post](/posts/diamond) for an overview of Diamond model.

WannaCry ransomware is a consequence of [MS-17-010](https://docs.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010), a vulnerability in Server Message Block V 1.0 which was exploited using [Eternal Blue exploit](https://en.wikipedia.org/wiki/EternalBlue). This caused a lot of impact to users with outdated version of windows OS and those who had not patched MS-17-010. 

Ransomware started to spread through a malicious email, a classic exploit delivery method. Once on network, lateral movement infected other systems which were vulnerable. 

---

## Applying Diamond model to this event,

### Core Features

* **Adversary** - Attacker with eternal blue exploit coupled with malware which encrypts data

* **Capability** - Encrypt Data, Move laterally exploiting vulnerability, 

* **Victim** - Windows OS vulnerable to MS-17-010

* **Infrastructure** - Mail, Malicious PDF

* **Socio-Political** - Money, attackers exploited users to make money. Intent was behind money. Fake promises were made to decrypt data once payment was made. 

* **Technical Meta feature** - Exploits MS-17-010 using eternal blue exploit and encrypts user data. Moves laterally infecting other systems through vulnerable SMB shares.

---

### Meta Features
* **Timestamp**      -  May 2017
* **Phase**               -   Exploit
* **Result**              -  Success ( impact on availability, user data is encrypted)
* **Direction**         -  Infrastructure to Victim
* **Methodology**   -  Phishing , Malware
* **Resources**        -  Vulnerable WindowsOS, Eternal blue exploit, malicious pdf

---

### Key Points:

* Adversary exploited victim for money.

* Adversary used phishing, malware methodology to attack victim

* Exploit occured in ~ May2017

* Key resources used, WindowsOS, Malicious PDF file, eternal blue exploit

* Adversary used capabilities like, eternal blue exploit, lateral movement through windows$shares, email to spread malware.

    * Encrypt data found in Victim system  

* End result - success in causing availability impact, encrypting user data.

---

[Part 1](/posts/diamond) of this blog explains Diamond model.

### References
1. [WannaCry ransomware](https://thehackernews.com/2017/05/how-to-wannacry-ransomware.html)





