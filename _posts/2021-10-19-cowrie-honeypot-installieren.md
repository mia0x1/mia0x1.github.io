---
title: Einen Honeypot mit cowrie installieren
categories: [opensource,linux,security]
description: cowrie ist ein open source Honeypot für SSH und Telnet, mit welchem sich Interaktionen von Angreifer:innen loggen und auswerten lassen.
tags:
 - opensource
 - linux
 - security
---

Ein Honeypot wird dazu eingesetzt, um Angreifer:innen auf ein bestimmtes Ziel (einen dafür präparierten Server) zu lenken, um zum Beispiel Angriffsmuster zu erkennen und Verhalten zu analysieren. So lassen sich mit Honeypots zum Beispiel erkennen, welche Malware gerade besonders verbreitet ist, wenn diese von den Angreifer:innen auf den Honeypot-Servern platziert werden. Mit [cowrie](https://github.com/cowrie/cowrie) hat man die Möglichkeit selber einen Honeypot aufzusetzen, denn cowrie ist frei verfügbar und Open Source. 

Cowrie simuliert ein UNIX System, auf welchen sich Angreifer:innen per SSH als root mit einem beliebigen Passwort (kommt auf die Konfiguration an) einloggen können. Dabei werden alle Interaktionen auf dem Server geloggt, welche anschließend ausgewertet werden können.


## Installation von Cowrie

In meiner Beispielinstallation für diesen Blogbeitrag verwende ich einen Server mit Ubuntu 21.04. Außerdem führe ich adminstrative Befehle als root aus. Solltet ihr in eurer Umgebung nicht als root eingeloggt sein, müsst ihr natürlich sudo vor die jeweiligen Befehle setzen, welche root-Privilegien benötigen. Nun zur Installation.

Als erstes aktualisieren wir unsere Paketquellen und installieren die neuesten Updates.

`apt update && apt upgrade -y`

Anschließend werden die benötigten Programme für cowrie installiert, unter anderem git und Python Virtual Environments.

`apt install git python3-virtualenv libssl-dev libffi-dev build-essential libpython3-dev python3-minimal authbind virtualenv`

Um dem Least Privilege-Prinzip gerecht zu werden, sollte Cowrie natürlich nicht unter dem root User laufen. Also wird ein neuer User angelegt, welcher auch keine sudo Berechtigungen erhält. 

`adduser --disabled-password cowrie`

Bevor wir zum User cowrie wechseln, ändern wir noch schnell den SSH-Port. Da cowrie später auf Port 22 lauschen soll, werden wir für SSH einen anderen Port vergeben. Die sshd_config könnt ihr mit einem Texteditor eurer Wahl bearbeiten.

`vim /etc/ssh/sshd_config`

In der Configfile den Port anpassen. Ich habe den Port 33333 verwendet.

{:refdef: style="text-align: center;"}
![Die sshd_config File, in welcher man den SSH Port ändern kann.]({{ site.baseurl }}/images/sshd_config.png)
{: refdef}

Danach noch den SSH-Service neu starten, damit die Änderung des Ports gültig wird.

`systemctl restart ssh.service`

Achtung: danach könnt ihr euch nur noch mit dem zuvor festgelegten Port per SSH mit eurem Server verbinden.

Nun wechseln wir zum User cowrie und laden alle notwendigen Dateien aus dem GitHub Repository runter.

`su - cowrie` 

`git clone http://github.com/cowrie/cowrie` 

`cd cowrie`

Wir sollten uns jetzt im Verzeichnis `/home/cowrie/cowrie` befinden. 
Als erstes erstellen wir dort die virtuelle Python-Umgebung. Diese wird benötigt, damit wir eine isolierte Arbeitsumgebung haben, welche keinen Einfluss auf globale Python-Module nimmt.

`virtualenv --python=python3 cowrie-env`

Jetzt muss die virtuelle Umgebung aktiviert und Abhängigkeiten installiert werden, welche in der `requirements.txt` definiert sind. Das man sich in der virtuellen Umgebung befindet, erkannt man an dem `(cowrie-env)` am Anfang der Kommandozeile.

`source cowrie-env/bin/activate` 

`pip install --upgrade pip` 

`pip install --upgrade -r requirements.txt`

Um die virtuelle Umgebung zu verlassen, gibt man einfach `deactivate` ein.

Cowrie verwendet standardmäßig den Port 2222. Damit cowrie wie vorhin angekündigt auf Port 22 lauschen kann, legen wir eine Regel in iptables an, damit Port 22 auf Port 2222 weitergeleitet wird. Dazu muss man allerdings wieder auf den root User wechseln.

`iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222`

Cowrie ist nun soweit bereit mit einer Standardkonfiguration gestartet zu werden. Dies macht man mit dem Befehl `bin/cowrie start`. Zum Stoppen des Honeypots gibt man einfach `bin/cowrie stop` ein.

Unter `cowrie/var/log/cowrie` liegt die Logdatei `cowrie.log`. In dieser Datei werden alle Ereignisse protokolliert, also zum Beispiel, wenn sich jemand in dem Honeypot einloggt und welche Befehle eingegeben wurden. Auf dem Screenshot ist ein Auszug aus dem Logfile zu sehen. Man sieht die IP-Adresse und welche Aktionen durchgeführt wurden. Also in diesem Fall wurde von mir als Test ein Ordner und dann eine Datei angelegt.

{:refdef: style="text-align: center;"}
![Auszug aus dem cowrie Logfile.]({{ site.baseurl }}/images/cowrie_log.png)
{: refdef}

Cowrie ist jetzt zwar funktionstüchtig, aber die Standardkonfiguration ist nur bedingt sinnvoll. Für Testzwecke natürlich völlig ausreichend, möchte ich an der Konfiguration ein paar sinnvolle Änderungen vornehmen. So lässt sich Cowrie in der Standardkonfiguration leicht als Honeypot identifizieren. Das sollte geändert werden. Nachzulesen ist dies in meinem [Beitrag](https://mialikescoffee.com/cowrie-konfigurieren-visualisieren/) zur cowrie Konfiguration und zur Visalisierung mit einem ELK Stack.

