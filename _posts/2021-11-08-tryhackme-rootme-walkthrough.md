---
title: TryHackMe RootMe writeup
description: Ein writeup zu dem TryHackMe Raum Rootme. Es kommen verschiedene Techniken und Tools, wie nmap und GoBuster, zum Einsatz.
tags:
 - security
 - linux
 - ctf
---

In diesem Beitrag zeige ich eine Lösungsweg für den TryHackMe Raum [RootMe](https://tryhackme.com/room/rrootme).
RootMe ist ein Raum, welcher sich an Einsteiger:innen richtet und mit ein wenig Vorwissen relativ schnell gelöst werden kann. Ziel ist es sich Zugang zu einem Server zu verschaffen und dort zwei Flags (Textdateien mit einem Lösungswort) zu finden. 

Eine Flag ist nur mit erhöhten Privilegien des Root Users zu erreichen, also muss man nicht nur den Zugang zum Server erlangen, sondern auch die Privilegien von einem normalen User zu root erhöhen.

## Vorwort

Vorweg der übliche Disclaimer: ihr könnt die Tools, welche hier vorgestellt werden für euer eigenes Netzwerk verwenden oder im Rahmen eines Penetration Tests einsetzen, wenn ihr dafür beauftragt wurdet. Jedoch dürft ihr diese Tools niemals gegen fremde Systeme ohne explizite Erlaubnis einsetzen.

## Aufklärung

Wir beginnen mit der Aufklärung und der ersten Informationsgewinnung. Hierfür nutzen wir nmap, um Informationen über offene Ports, Dienste und das Betriebssystem zu erlangen. 

Wir nutzen folgenden Befehl, mit welchem der Output auch in eine Textdatei ausgegeben wird.

`nmap -A $ipaddress -o nmap.txt`

{:refdef: style="text-align: center;"}
![Ergebnis des nmap Scans]({{ site.baseurl }}/images/nmap_rootme.png)
{: refdef}

Folgende Ergebnisse lieferte der Scan

* Port 22 für SSH ist offen
* Port 80 ebenfalls offen. Dahinter läuft ein Apache Webserver in der Version 2.4.29
* Das Betriebssystem ist wahrscheinlich ein Ubuntu Linux

## Untersuchung des Webservers

Öffnen wir die Webseite über unseren Browser erscheint ein dynamischer Schriftzug mit der Frage "Can you root me?"

{:refdef: style="text-align: center;"}
![Die Webseite von rootme]({{ site.baseurl }}/images/webseite_rootme.png)
{: refdef}

Weitere Informationen finden wir dort nicht. Quelltext und Developer Tools sind uns an dieser Stelle auch keine Hilfe.

Deswegen werfen wir Gobuster an und scannen die Directories des Webservers.
Als Wörterliste habe ich die directory-list-2.3-small.txt von [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/directory-list-2.3-small.txt) auf GitHub verwendet. 

`gobuster -u http://$ip -w /opt/wordlists/directory-list-2.3-small.txt`

Der Gobuster Scan hat sich gelohnt: Wir finden den Ordner /panel/, hinter welchem sich ein Uploadformular versteckt. Dies werden wir sicherlich irgendwie ausnutzen können, um uns Zugang zum Server zu verschaffen.  

{:refdef: style="text-align: center;"}
![Das Uploadformular auf der Webseite der Vuln University]({{ site.baseurl }}/images/upload_rootme.png)
{: refdef}

## Uploadformular ausnutzen

Als Erstes habe ich gedacht, dass man sich einen Zugang zum Server mit einer php reverse shell verschaffen könnte. Das funktioniert aber auf den ersten Blick nicht, weil die Webseite den Upload des Dateiformats .php verbietet, was anscheinend serverseitig geprüft wird.

{:refdef: style="text-align: center;"}
![Die Webseite verbietet Uploads von .php Dateien.]({{ site.baseurl }}/images/php_rootme.png)
{: refdef}

giAllerdings gibt es mehrere php extensions. So werden zum Beispiel Uploads mit der extension .phtml akzeptiert. Also laden wir eine php reverse shell (z.B. die php-reverse-shell von [pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)) mit der Dateiendung .phtml über das Uploadformular auf den Server und führen diese aus, in dem wir die URL der Shell aufrufen: http://$ip/uploads/shell.phtml .
Im Quelltext der reverse shell muss lediglich IP-Adresse und Port angepasst werden, wo wir natürlich unsere IP-Adresse eintragen.

{:refdef: style="text-align: center;"}
![Die Webseite verbietet Uploads von .php Dateien.]({{ site.baseurl }}/images/phpshell_rootme.png)
{: refdef}

Wir öffnen auf dem gleichen Port eine Verbindung mit netcat, worüber sich die reverse shell auf unsere Maschine verbindet. `nc -lvnp 4444`

Nun haben wir eine shell auf dem Server als User www-data. Unter /var/www finden wir die erste Flag.

{:refdef: style="text-align: center;"}
![Erste Flag gefunden.]({{ site.baseurl }}/images/ersteflag_rootme.png)
{: refdef}


## Privilege Escalation

Um an die Rootflag zu kommen, müssen wir unsere Privilegien erhöhen. Dazu suchen wir eine Datei mit gesetztem SUID Bit, welches wir ausnutzen können. SUID steht für Set User ID und bedeutet, dass eine Datei auch mit den Rechten des Users ausgeführt wird, welchem sie gehört. Wenn also eine Datei root gehört und das SUID Bit gesetzt ist, kann dies eine Schwachstelle für eine mögliche Privilege Escalation sein. Mit find kann man alle Dateien mit SUID Bit finden: `find / -type f -perm -04000 -ls 2>/dev/null`. 

Im Ergebnis der Suche ist zu sehen, dass Python SUID Berechtigungen hat. Bei [GTFOBins](https://gtfobins.github.io/gtfobins/python/) finden wir heraus, dass wir mit Python und SUID eine root Shell bekommen können. Dies funktioniert mit dem Befehl `python -c 'import os; os.execl("/bin/sh", "sh", "-p")'`. Und siehe da: Wir sind root. Unter /root/ finden wir schließlich die Rootflag. 

{:refdef: style="text-align: center;"}
![Erste Flag gefunden.]({{ site.baseurl }}/images/rootflag_rootme.png)
{: refdef}
