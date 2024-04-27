---
published: true
description: A writeup for TryHackMe Startup. This is a easy-level CTF challenge on tryhackme.com
tags:
  - security
  - ctf
  - linux
title: TryHackMe Startup Writeup
header:
 image: /images/header/startup.webp
 teaser: /images/header/startup.webp
---

This is a writeup for TryHackMe [Startup](https://tryhackme.com/room/startup).

We are Spice Hut, a new startup company that just made it big! We offer a variety of spices and club sandwiches (in case you get hungry), but that is not why you are here. To be truthful, we aren't sure if our developers know what they are doing and our security concerns are rising. We ask that you perform a thorough penetration test and try to own root. Good luck!
{: .notice--info}

## Preface

First, the usual disclaimer: you can use the tools presented here for your own network or during a penetration test if you've been authorized to do so. However, you must never use these tools against external systems without explicit permission.
{: .notice--warning}

## Enumeration

First, we scan the server with nmap for open ports and use switches for Script Scan and Version Detection.

```
nmap -sV -sC $ip
```

We find that three different services are running on the server:

- 21 FTP
- 22 SSH
- 80 http / Apache

Upon closer inspection, it is noted that the FTP server allows anonymous access. Additionally, nmap kindly indicates that there is a folder with write permissions on the FTP.

![]({{site.baseurl}}/images/startup01.png)

When accessing the website in the browser, only a note appears stating that the site is currently under development.

If we enumerate the web server for directories, we find the directory /files. Very convenient, as that's where the folder we can write to via FTP is located.

![]({{site.baseurl}}/images/startup02.png)

![]({{site.baseurl}}/images/startup03.png)

## Initial Access

So, it's relatively obvious to gain initial access by loading a reverse shell via FTP onto the server and then accessing it through the browser to execute the shell server-side and connect it to our listener.

We log in to the FTP server with anonymous / anonymous and navigate to the ftp folder. There, we can upload our reverse shell. You can use a PHP reverse shell, which you can find, for example, at [Pentest Monkey](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet).

![]({{site.baseurl}}/images/startup05.png)  

![]({{site.baseurl}}/images/startup04.png)

First, we start a Netcat listener:

```bash
nc -lvnp 1234
``` 

After that we execute the reverse shell, which will then connect to our listener.

Now we have a shell as the user www-data.

## The Secret Ingredient
In the root directory, we find the recipe.txt, which reveals the secret ingredient for the soup.

![]({{site.baseurl}}/images/startup06.png)

## User Flag

In the directory /incidents, we find a pcap-ng file. We can simply download this using our browser by copying it to the web server's directory.

```bash
cp suspicious.pcapng /var/www/html/files
```

With Wireshark, we can open the .pcapng file. When we analyze the network traffic, we see that someone in the past gained access to the server with a reverse shell. Upon closer examination of the TCP streams, we find the password for the user lennie.
![]({{site.baseurl}}/images/startup08.png)

We can log in as lennie with SSH on the server and find the user flag in the user's home directory.

![]({{site.baseurl}}/images/startup09.png)

## Root Flag

Under `/home/lennie/scripts/`, we find the shell script planner.sh.
The script belongs to root and can be executed by our user.

```bash
#!/bin/bash
echo $LIST > /home/lennie/scripts/startup_list.txt
/etc/print.sh
```

We see that the script calls another script named print.sh. This script belongs to the user lennie and can therefore be edited by us. We modify it so that the root flag is copied to /tmp/root.txt.

```bash
#!/bin/bash
cp /root/root.txt /tmp/root.txt
```

Now we just need to execute planner.sh to subsequently find the root flag under /tmp.

![]({{site.baseurl}}/images/startup10.png)