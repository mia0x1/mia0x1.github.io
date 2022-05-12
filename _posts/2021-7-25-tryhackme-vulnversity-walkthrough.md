---
title: TryHackMe Vulnversity writeup
description: Ein writeup zu dem TryHackMe Raum Vulnversity. Es kommen verschiedene Techniken und Tools, wie nmap und GoBuster, zum Einsatz.
tags:
 - security
 - linux
 - ctf
---

Dieser Beitrag ist ein writeup für den TryHackMe Raum [Vulnversity](https://tryhackme.com/room/vulnversity). Für alle, die das hier lesen und noch nicht von TryHackMe gehört haben: dabei handelt es sich um eine Online-Lernplattform für IT-Security, in welcher in interaktiven Labs und praktischen Übungen verschiedene Angriffs- und Verteidigungstechniken gelehrt werden.

Für Vulnversity werden verschiedene Konzepte und Techniken aus den Bereichen Aufklärung/Enumeration, Angriff von Web Applications über einen File Upload und Privilege Escalation genutzt. Ziel ist es sogenannte Flags zu bekommen. Das sind einfach nur Textdateien, die sowas wie ein Lösungswort enthalten und irgendwo auf dem anzugreifenden Server abgespeichert sind.

## Vorwort: verwendete Tools 

Die folgenden Tools kommen bei der Lösung der Vulnversity zum Einsatz. Alle diese Tools solltet ihr entweder nur auf euren eigenen Systemen verwenden oder auf solchen Systemen, wofür euch explizit die Erlaubnis erteilt wurde.

* nmap
* Burp Suite
* GoBuster
* php-reverse-shell
* GTFObins

## Aufklärung

Wir beginnen mit der Informationsgewinnung und Aufklärung. Hierfür nutzen wir nmap, um Informationen über offene Ports, Dienste und das Betriebssystem zu erlangen. 

Wir nutzen folgenden Befehl, mit welchem der Output auch in eine Textdatei ausgegeben wird.

`nmap -sC -sV $ipaddress -o nmap.txt`

{:refdef: style="text-align: center;"}
![Ergebnis des nmap Scans]({{ site.baseurl }}/images/nmap.png)
{: refdef}

Zwei Erkenntnisse aus dem nmap Scan:

* 6 offene Ports für verschiedene Dienste
* Ein Apache Webserver läuft auf dem Port 3333

## Untersuchung des Webservers

Die Webseite, welche der Webserver bereitstellt, können wir über die IP-Adresse und den Port 3333 im Browser aufrufen.

{:refdef: style="text-align: center;"}
![Die Webseite der Vuln University]({{ site.baseurl }}/images/webseite.png)
{: refdef}

Auf der Webseite erfahren wir einige Informationen über die "Vuln University", aber nichts was uns auf der Suche nach der Flag weiterbringen würde.

Mit GoBuster können wir mithilfe einer Wörterliste Ordner und Dateien auf dem Webserver finden. Als Wörterliste habe ich die directory-list-2.3-small.txt von [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/directory-list-2.3-small.txt) auf GitHub verwendet. 

`gobuster -u http://$ip:3333 -w /opt/wordlists/directory-list-2.3-small.txt`

{:refdef: style="text-align: center;"}
![Ergebnis des GoBuster Scans]({{ site.baseurl }}/images/gobuster.png)
{: refdef}

Durch den GoBuster Scan haben wir den Ordner /internal gefunden. Es stellt sich heraus, dass sich dahinter ein Uploadformular verbirgt.

{:refdef: style="text-align: center;"}
![Das Uploadformular auf der Webseite der Vuln University]({{ site.baseurl }}/images/uploadformular.png)
{: refdef}

## Exploit des File Uploads

Es stellt sich die Frage, wie wir die Uploadfunktion ausnutzen können, um uns Zugang zum Server zu verschaffen. Um herauszufinden, welche Dateiformate wir auf den Server hochladen können, verwenden wir Burp Suite. Dabei kann wieder eine Wörterliste von SecLists genutzt werden, dieses Mal die [web-extensions.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt)

Wir laden eine Datei über das Uploadformular hoch und fangen die HTTP-Post Anfrage mit dem Burp Suite Proxy ein. Diese senden wir dann mit einem Rechtsklick an den Intruder.

{:refdef: style="text-align: center;"}
![Der BurpSuite Proxy fängt den Post des Uploads ab.]({{ site.baseurl }}/images/burp_proxy.png)
{: refdef}

Anschließend zum Intruder wechseln. Dort wird das Angriffsziel festgelegt (IP-Adresse und Port des Webservers eintragen). Anschließend in den Positions beim filename die Dateierweiterung eintragen und in § einschließen. 

{:refdef: style="text-align: center;"}
![Im Burp Intruder wird die Abfrage so konfiguriert, dass die Dateiendungen enumeriert werden können.]({{ site.baseurl }}/images/burp_intruder.png)
{: refdef}

Im Reiter Payloads laden wir die Wörterliste web-extensions.txt hoch und entfernen den Haken vor "URL-encode these characters". Anschließend können wir den Angriff über den Button "Start attack" ausführen. 

Burp Suite probiert nun alle Dateierweiterungen aus der Liste mit dem Uploadformular aus. Der Webserver gibt in fast allen Fällen "Extension not allowed" zurück. Für .phtml bekommen wir einen "Success". Wir sind also in der Lage .phtml Dateien auf den Webserver hochzuladen.

{:refdef: style="text-align: center;"}
![Für das Format .phtml gibt es einen Success.]({{ site.baseurl }}/images/burp_success.png)
{: refdef}

## Zugang zum Server mit Reverse Shell

Um Zugang zum Server zu erlangen, wird die [php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) von pentestmonkey verwendet. An der Datei müssen nur zwei kleine Änderungen vorgenommen werden:

* IP-Adresse der Maschine eintragen, mit welcher wir über VPN mit TryHackMe verbunden sind und einen Port auswählen.
* Dateiendung in .phtml ändern.

Die Datei laden wir über das Uploadformular auf den Webserver. 
Mit netcat warten wir auf dem vorher festgelegten Port auf die Verbindung.
Die reverse-shell wird auf dem Server ausgeführt, in dem wir die URL `http://$IP:3333/internal/uploads/php-reverse-shell.phtml` im Browser aufrufen. 

{:refdef: style="text-align: center;"}
![In der Reverse Shell wird die richtige IP und der Port eingetragen.]({{ site.baseurl }}/images/php_reverse_shell.png)
{: refdef}

{:refdef: style="text-align: center;"}
![Wir bekommen eine Shell über netcat.]({{ site.baseurl }}/images/netcat.png)
{: refdef}

Wir haben es geschafft und haben eine Shell auf dem Server. Unter /home/bill finden wir eine user.txt, welche die erste Flag enthält.

{:refdef: style="text-align: center;"}
![Erste Flag gefunden.]({{ site.baseurl }}/images/erste_flag.png)
{: refdef}

## Privilege Escalation

Um an die zweite Flag zu kommen, müssen wir unsere Privilegien erhöhen, da diese nur für root zugänglich ist. Dazu nutzen wir die SUID-Berechtigungen. SUID steht für "set owner userID upon execution". Das bedeutet, dass ein Programm mit den Berechtigungen des Besitzers der Datei ausgeführt wird und nicht mit den Berechtigungen des Users, der die Datei ausführt. Wir suchen also eine Datei, welche mit root Berechtigungen ausgeführt wird. Mit `find / -perm /4000 2>/dev/null` finden wir heraus, dass /bin/systemctl die SUID Berechtigung hat. 

{:refdef: style="text-align: center;"}
![systemctl hat die SUID Berechtigung.]({{ site.baseurl }}/images/systemctl.png)
{: refdef}

Mit systemctl lässt sich ein Systemservice erstellen, welcher mit root Rechten läuft. 

Bei [GTFObins](https://gtfobins.github.io/gtfobins/systemctl/) findet man ein Beispiel, wie man mit systemctl und SUID Privilegien erhöhen kann.

Da TryHackMe schon verraten hat, dass die Flag unter /root/root.txt liegt, können wir diese einfach auslesen und an einen Ort kopieren, welche für unseren Useraccount lesbar ist. In unserem Fall legen wir die Flag unter /tmp/flag ab. Dazu wird über systemctl der Befehl `cat /root/root.txt > /tmp/flag` mit Rootrechten ausgeführt.

{:refdef: style="text-align: center;"}
![Mithilfe der GTFObins kommen wir an die Root Flag.]({{ site.baseurl }}/images/rootflag.png)
{: refdef}

Das ist nur eine Möglichkeit, um die Privilegien zu erhöhen. Einen weiteren coolen Ansatz habe ich hier gefunden: [https://gist.github.com/A1vinSmith/78786df7899a840ec43c5ddecb6a4740](https://gist.github.com/A1vinSmith/78786df7899a840ec43c5ddecb6a4740)
Hier wird ein Payload geschrieben, welcher einen Service ausführt, über den man dann eine reverse-shell mit Rootrechten bekommt. So könnte man direkt über die Shell auf den Ordner /root/ zugreifen und die Flag auslesen.

Ich hoffe ihr hattet Spaß beim Lesen. Vielleicht konnte ich ja sogar jemandem weiterhelfen. Mir persönlich hat die Lösung der Vulnversity sehr viel Spaß gemacht, weswegen ich auch diesen Walkthrough verfasst habe.
