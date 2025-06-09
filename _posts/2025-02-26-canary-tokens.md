---
published: true
description: Deception technology plays a crucial role in modern cybersecurity by helping organizations detect and respond to threats more effectively. With Canarytokens, anyone can easily implement deception techniques to enhance their network security.
tags:
  - security
title: Catching Attackers with Deception - A Hands-On Guide to Canarytokens
header:
 image: /images/header/header_laptop.jpg
 teaser: /images/header/header_laptop.jpg
---

Deception technology plays a crucial role in modern cybersecurity by helping organizations detect and respond to threats more effectively. Since no security system is entirely foolproof, it is vital to consider what happens after an attacker gains initial access. Deception techniques can help defenders to detect adversaries early, thereby limiting their ability to move laterally. Earlier detection will also reduce the likelihood that significant damage occurs.

At its core, deception involves deploying false IT assets within a network. They act as decoys to attract and expose attackers. Deceptive decoys can mimic legitimate systems, files, credentials, and other IT assets. They lead attackers into a monitored environment where their actions are generating noise and can be detected and analyzed. The intelligence gathered from these interactions helps security teams strengthen their defenses.

By integrating deception technologies into their security strategy, organizations enhance their ability to recognize and mitigate threats before they escalate into security breaches, such as ransomware attack or data exfiltration.

Deception technology can be categorized into varius types, with two of the most commong being **honeypots** and **honey users**. ([Learn More](https://www.rapid7.com/fundamentals/deception-technology/)).

### Honeypots

A **honeypot** is a decoy computer system deployed within a network, positioned alongside production machines. Honeypots serve multiple purposes:

* **Diverting malicious traffic** away from critical systems
* **Gathering intelligence** on attacker behavior and techniques
* **Strengthening security** by using the insights gained to harden defenses

If this concept excites you, you can experiment by setting up your own honeypot. A cost-effective way to start would be to rent a cheap VPS and deploy a honeypot in the cloud.
I previously wrote an [article on hosting T-Pot](https://mialikescoffee.com/t-pot-honeypot/), a collection of different honeypots developed by T-Systems—definitely worth checking out!

### Honey Users

A **honey user** is a decoy account created in Active Directory (AD) with no special privileges.
These fake accounts can help detect AD enumeration and password-guessing attacks.
To be effective, a honey user should have an enticing username, such as "BackupAdmin" or "SuperUser", that might attract an attacker's interest..

By monitoring login attempts and access to these accounts, security teams can quickly identify malicious activity.

## What are Canary Tokens?

When it comes to deception, a free and easy-to-use tool for defenders is [Canarytokens](https://canarytokens.org/nest/).

Canarytokens are digital tripwires that can be deployed in various locations across your network. The core idea is to lure attackers into triggering them.

For example there is a **Canarytoken DOCX file** that could be named "top_secret.docx" to spark curiosity.
If an attacker opens the file, instead of finding sensitive information, an alert is triggered - sending an email to the **Security Operations Center (SOC)** or generating an alert to the **SIEM (Security Information and Event Management)** system. 

The possible applications for Canarytokens are endless. This makes them a versatile addition to your security arsenal.

![Screenshot of the Canarytokens website]({{site.baseurl}}/images/canarytokens.png)

## Case Study: Deploying a Canary Token

After covering the theory, let's dive into a practical example of how you can deploy your own Canarytoken.

To start, visit the [Canarytokens Website](https://canarytokens.org/nest/) and explore different token variations. You might find options that integrate into your infrastructure, enhancing your defense-in-depth strategy.

### Using A DNS Canarytoken

A **DNS Canarytoken** is a special DNS name that triggers an alert whenever it is resolved. Here are two ways it can be effectively deployed:

1. **Embedding in sensitive files** - Place the fake DNS name into critical files like `.bashrc` or `.bash_history`. Many reconnaissance tools (e.g., nmap or Recon-ng) automatically collect and resolve DNS names during enumeration. If an attacker uses such a tool that resolves the DNS name, an alert will be triggered.

2. **Inserting into internal documentation** - If an attacker accesses internal documentation and attempts to resolve the embedded fake DNS name, the same detection mechanism is activated.

To demonstrate, I created a DNS token and stored it inside a simple shell script.

```sh
#!/bin/bash

# Running this script will resolve the DNS canarytoken and trigger an email alert

dig yourtoken.canarytokens.com
```

When this script is executed - or if an attacker scrapes and resolves the DNS name - the Canarytoken is triggered.

![Screenshot of the email alert after triggering the DNS token]({{site.baseurl}}/images/canarytoken_alert.png)

After running the script, I immediately received an email alert, proving that the Canarytoken successfully detected unauthorized activity.

## Conclusion

Deception technologies such as honeypots, honey users, and Canarytokens provide a proactive approach to cybersecurity. By luring attackers into interacting with fake assets, security teams gain valuable intelligence while reducing the risk of undetected intrusions.

Have you ever considered using Canarytokens or a similar product or have you already implemented it in the past? Please drop me your thoughts. :)