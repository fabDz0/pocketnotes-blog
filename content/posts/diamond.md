---
title: "Diamond Model - Intrusion Analysis"
date: 2020-06-20T23:35:12+05:30
draft: false
tags: ["threatIntel", "diamond_model", "Intrusion_analysis", "notes"]
---

![Diamond Model](/img/diamondModel.jpg)

Diamond model for intrusion analysis is used to analyze threats. It helps to set a structure and flow to threats so that analysts can control its effects. It may not be complete but sufficient enough to control the impact.<!--more-->

I particularly found this model interesting to analyze attack and summarize tactics. Once we know how the model is organized, then a diagram will summarize entire operation.

Diamond model of intrusion analysis breaks down an intrusion (event) to core and meta features. The basic element of Diamond model is "event" , A diamond event is described with 4 core features and 6 meta-features.

Core features are Adversary, Capability, Infrastructure and Victim. These are nodes in Diamond model. A typical flow would be like, Adversary deploying a capability over some infrastructure against a victim. Just like an attacker deploying a malware on email server to steal user conversations.

Core features are complemented with meta features, timestamp, phase, result, direction, methodology and resources. Meta features in this model is not for completeness but for sufficiency. If there are other features which add more details to model those can be included. But to start with, a predefined list is provided.

Core features and meta feature's information is associated with a confidence value. A value to determine reliability of information.

Lets explore core features,
* **ADVERSARY** - attacker who uses a certain capability to exploit victim. Adversary operator conducts the intrusion activity. Adversary customer stands benefit from the intrusion activity.
* **CAPABILITY** - Physical or logical device used by adversary to  deliver a capability or control a capability and exploit victim.
* **Infrastructure** - Physical or logical communication structure used to deliver a capability.
    * Type 1 Infrastructure - Fully controlled by adversary
    * Type 2 Infrastructure - controlled by intermediate devices (zombies, bots)
* **Victim** - adversary target. It can be an organization, a person, email address, IP address anything.
Victim persona is organization name, people, job roles, interests , etc.
Victim asset is the actual asset where attack happens. Email accounts, IP address, social media profiles, etc

---
Let's understand default list of Meta-features, these are not the only meta features to an event, but serves as a good starting point.

**TIMESTAMP** - time/date of event
**PHASE** - This relates to the current phase of intrusion according to cyber kill chain (lockheed martin cyber kill chain). According to [author](http://www.activeresponse.org/wp-content/uploads/2014/05/Diamond_Poster.pdf) every activity has two phases which must be successful to get needed result.
**RESULT**  - Success/Failed/Unknown, affect to CIA (confidentiality, integrity, availability) due to intrusion
**DIRECTION** - direction in which event is happening.
Possible directions,

* Infrastructure  - Victim
* Victim - Infrastructure
* Infrastructure - Infrastructure
* Adversary - Infrastructure 
* Infrastructure - Adversary
* Unknown

**METHODOLOGY**  - spearphish, malware

**RESOURCE**    - all resources on which event, core and meta feature depends on.

---

In Second part of the blog will use Diamond Model to analyse existing threat.

---
## References
1. [Youtube video](https://www.youtube.com/watch?v=0QHUS8SNTNc)
2.  [Diamond model summary](http://www.activeresponse.org/wp-content/uploads/2013/07/diamond_summary.pdf)
3. [Research paper](http://www.activeresponse.org/wp-content/uploads/2014/05/Diamond_Poster.pdf)



