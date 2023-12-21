---
published: true
description: Ein writeup zum TryHackMe Raum Overpass.
tags:
  - security
  - ctf
  - linux
title: TryHackMe Startup
header:
 image: /images/header/startup.webp
 teaser: /images/header/startup.webp
 image_description: "Ein Screenshot der Wallabag-Artikelliste."
---

Dies ist ein writeup zum TryHackMe Raum [Startup](https://tryhackme.com/room/startup).

We are Spice Hut, a new startup company that just made it big! We offer a variety of spices and club sandwiches (in case you get hungry), but that is not why you are here. To be truthful, we aren't sure if our developers know what they are doing and our security concerns are rising. We ask that you perform a thorough penetration test and try to own root. Good luck!
{: .notice--info}

## Vorwort

Vorweg der übliche Disclaimer: ihr könnt die Tools, welche hier vorgestellt werden für euer eigenes Netzwerk verwenden oder im Rahmen eines Penetration Tests einsetzen, wenn ihr dafür beauftragt wurdet. Jedoch dürft ihr diese Tools niemals gegen fremde Systeme ohne explizite Erlaubnis einsetzen.
{: .notice--warning}

## Enumerieren

Als erstes Scannen wir den Server mit nmap nach offenen Ports und benutzen die Switches für Script Scan und Version Detection.

`nmap -sV -sC $ip`

Wir finden heraus, dass drei verschiedene Dienste auf dem Server laufen:

- 21 FTP
- 22 SSH
- 80 http / Apache

Bei genauerem Hinsehen fällt auf, dass der FTP-Server anonymen Zugang erlaubt.
Außerdem zeigt nmap netterweise bereits an, dass es auf dem FTP einen Ordner mit Schreibberechtigungen gibt.

![]({{site.baseurl}}/images/startup01.png)

Ruft man die Website im Browser auf, erscheint nur ein Hinweistext, dass sich die Seite gerade in der Entwicklung befindet.

Wenn wir den Webserver nach Directories enumerieren, finden wir /files. Sehr praktisch, denn dort liegt auch der Ordner, den wir per FTP beschreiben können.

![]({{site.baseurl}}/images/startup02.png)

![]({{site.baseurl}}/images/startup03.png)
## Initial Access

Um uns initial Zugang zu verschaffen, ist es also relativ offensichtlich eine Reverse Shell via FTP auf den Server zu laden und diese dann über den Browser aufzurufen, damit die Shell serverseitig ausgeführt wird und sich mit unserem Listener verbindet.

Wir loggen uns auf dem FTP Server mit anonymous / anonymous ein und wechseln zum Ordner ftp.
Dort können wir unsere Reverse Shell hochladen. Dazu könnt ihr eine PHP Reverse Shell verwenden, welche ihr z.B. bei [Pentest Monkey](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)findet.

![]({{site.baseurl}}/images/startup05.png)  

![]({{site.baseurl}}/images/startup04.png)

Als erstes startet wir den Listener auf unserer Mashcine mit `nc -lvnp 1234` und führen anschließend die Reverse Shell aus, welche sich dann zu unserem Listener verbindet.

Nun haben wir eine Shell als User www-data.

## Die geheime Zutat
Im Root Directory finden wir die recipe.txt, welche uns die geheime Zutat für die Suppe verrät.

![]({{site.baseurl}}/images/startup06.png)

## User Flag

Im Verzeichnis /incidents finden wir eine pcap-ng Datei. Wir können diese einfach mit unserem Browser runterladen, in dem wir sie in das Verzeichnis des Webservers kopieren.

`cp suspicious.pcapng /var/www/html/files`

Mit Wireshark können wir die .pcapng File öffnen. Wenn wir den Netzwerktraffic analysieren, sehen wir, dass sich jemand in der Vergangenheit Zugriff auf den Server mit einer Reverse Shell verschafft hat. 
Bei genauerer Betrachtung der TCP Streams finden wir dann das Passwort für den User lennie.
![]({{site.baseurl}}/images/startup08.png)

Wir können uns als lennie mit SSH auf dem Server einloggen und finden im Home-Verzeichnis des Users die User flag.

![]({{site.baseurl}}/images/startup09.png)
## Root Flag

Unter `/home/lennie/scripts/`finden wir das Shellskript planner.sh.
Das Skript gehört root und kann von unserem User ausgeführt werden.

```
#!/bin/bash
echo $LIST > /home/lennie/scripts/startup_list.txt
/etc/print.sh
```

Wir sehen, dass das Skript ein weiteres Skript namens print.sh aufruft. 
Diese Skript gehört dem User lennie und kann daher von uns editiert werden.
Wir ändern es so ab, dass die root flag nach /tmp/root.txt kopiert wird.

```
#!/bin/bash
cp /root/root.txt /tmp/root.txt
```

Nun müssen wir nur planner.sh ausführen, um anschließend die root flag unter /tmp zu finden.  

![]({{site.baseurl}}/images/startup10.png)