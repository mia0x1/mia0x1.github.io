---
published: true
description: Ein writeup zum TryHackMe Raum Overpass.
tags:
  - security
  - linux
  - ctf
header:
  image: /images/header/overpass.webp
  teaser: /images/header/overpass.webp
  image_description: Eine Hackerin sitzt auf dem Sofa vor ihrem Laptop und löst das Overpass CTF. Im Hintergrund sieht man die Stadt bei nacht durch das große Fenster.
---
Ein writeup zum TryHackMe-Raum [Overpass](https://tryhackme.com/room/overpass)

**Overpass:** What happens when some broke CompSci students make a password manager?
{: .notice--info}

## Disclaimer

Vorweg der übliche Disclaimer: ihr könnt die Tools, welche hier vorgestellt werden für euer eigenes Netzwerk verwenden oder im Rahmen eines Penetration Tests einsetzen, wenn ihr dafür beauftragt wurdet. Jedoch dürft ihr diese Tools niemals gegen fremde Systeme ohne explizite Erlaubnis einsetzen.

## Ziel

Zwei Flags müssen gefunden werde - eine User- und eine Rootflag.

* Hack the machine and get the flag in user.txt 
* Escalate your privileges and get the flag in root.txt

## Aufklärung

Als erstes machen wir einen Portscan mit nmap.

`nmap -A $ipaddress -o nmap.txt`

{:refdef: style="text-align: center;"}
![Ergebnis des nmap Scans]({{ site.baseurl }}/images/overpass_nmap.png)
{: refdef}

Wir finden einen offenen SSH-Server (Port 22) und einen Webserver (Port 80).

## Untersuchung des Webservers

Auf der Website wird Overpass vorgestellt - ein sicherer Passwortmanager.

{:refdef: style="text-align: center;"}
![Die Webseite von Overpass]({{ site.baseurl }}/images/overpass_website.png)
{: refdef}

Mit ffuf scannen wir den Webserver nach Ordnern und finden unter /admin ein Loginformular.

{:refdef: style="text-align: center;"}
![Ergebnis des ffuf directory scans]({{ site.baseurl }}/images/overpass_ffuf.png)
{: refdef}

{:refdef: style="text-align: center;"}
![Loginforumlar auf der Adminseite]({{ site.baseurl }}/images/overpass_admin.png)
{: refdef}

Über die Developer Tools des Browsers untersuchen wir Java Script, welches beim Login ausgeführt wird. Die Datei heißt login.js.

Bei genauerem Hinsehen fällt auf, dass die Abfrage von Username und Passwort übersprungen wird, wenn ein Cookie mit dem Key SessionToken und beliebigem Value gesetzt ist.

{:refdef: style="text-align: center;"}
![JavaScript-Code Ausschnitt. Wenn SessionToken gesetzt ist, wird Passwortabfrage übersprungen.]({{ site.baseurl }}/images/overpass_js.png)
{: refdef}

Wir verifizieren dies, indem wir in den Developer Tools unter Storage einen Cookie mit dem Namen SessionToken und beliebigem Value setzen. Anschließend laden wir die Seite einmal neu. Wir erhalten einen SSH-Key von James.

{:refdef: style="text-align: center;"}
![JavaScript-Code Ausschnitt. Wenn SessionToken gesetzt ist, wird Passwortabfrage übersprungen.]({{ site.baseurl }}/images/overpass_sshkey.png)
{: refdef}

## SSH-Verbindung aufbauen

Wir speichern den Key in einer Textdatei. Da dieser den Status Encrypted hat, müssen wir das Passwort cracken. Ohne das Passwort können wir uns mit dem Key nicht am Server authentifizieren. Dazu extrahieren wir den Hash mit ssh2john. 

```
ssh2john id_rsa > id_rsa.hash
```

Anschließend könnten wir den Hash mit John the Ripper cracken.

```
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash
```
Wenn John den Hash gecrackt hat, lassen wir uns das Passwort für den SSH-Key anzeigen.

``` 
john id_rsa.hash --show
``` 
Mit dem Passwort und dem SSH-Key können wir uns nun als User james per SSH am Server authentifizieren. Dort finden wir auch die Userflag.

{:refdef: style="text-align: center;"}
![Authentifizierung am Server via SSH und Anzeigen der Userflag.]({{ site.baseurl }}/images/overpass_userflag.png)
{: refdef}

## Privilege Escalation

Für die Rootflag müssen wir unsere Privilegien erhöhen. Als geeigneten Angriffsvektor finden wir einen Cronjob unter /etc/crontab, welcher als root ausgeführt wird. Dieser lädt jede Minute ein buildscript von einer URL, welches durch eine Pipe an Bash übergeben wird. Um eine Shell als root zu bekommen, müssen wir das Skript durch eine Reverse Shell ersetzen.

{:refdef: style="text-align: center;"}
![Anzeige des Cronjobs, welcher minütlich das Buildscript neu lädt und an Bash übergibt.]({{ site.baseurl }}/images/overpass_cronjob.png)
{: refdef}

Dazu erstellen wir als erstes die Reverse Shell, welche wir buildscript.sh nennen und im Ordner /downloads/src abspeichern, um der Struktur im Cronjob zu entsprechen.

{:refdef: style="text-align: center;"}
![Python Reverse Shell.]({{ site.baseurl }}/images/overpass_revshell.png)
{: refdef}

Wenn der Cronjob die Reverse Shell ausführt, verbindet sich diese zu unserem Listener auf dem Port 4242. 

Als nächstes müssen wir /etc/hosts bearbeiten. Der Cronjob lädt das Skript mit curl von der Domain overpass.thm herunter. Wir passen die Hostfile so an, dass overpass.thm auf unsere IP-Adresse zeigt. Dazu ersetzen wir die 127.0.0.1 in der Zeile mit overpass.thm durch unsere eigene IP-Adresse.

Damit die Reverse Shell von unserer Maschine heruntergeladen wird, starten wir einen HTTP-Server mit Python im Wurzelverzeichnis, wo sich auch der Ordner downloads befindet.
```
python3 -m http.server 80
```

Außerdem starten wir einen netcat-Listener, damit sich die Reverse Shell zu unserer Maschine verbinden kann.

```
nc -lvnp 4242
```
Durch den Cronjob, welcher die Reverse Shell von unserer Maschine herunterlädt und mit Bash ausführt, wird die Verbindung zu unserem netcat-Listener aufgebaut. 
Mit whoami verifizieren wir, dass wir Root sind und zeigen uns dann die Rootflag an.

{:refdef: style="text-align: center;"}
![Anzeige der rootflag.]({{ site.baseurl }}/images/overpass_rootflag.png)
{: refdef}





