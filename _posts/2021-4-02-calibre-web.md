---
title: Calibre-web - ein schickes Webfrontend für eure E-books
description: Calibre-web ist eine ansehnliche web-basierte App, mit denen ihr eure E-Book Sammlung verwalten könnt.
tags:
 - opensource
 - selfhosting
 - linux
---

Calibre-web ist eine ansehnliche web-basierte App, mit denen ihr eure E-Book Sammlung verwalten könnt. Ihr könnt damit durch eure Bücher stöbern, diese direkt über den Browser lesen oder auf eines eurer Geräte herunterladen. Für den Download bieten sich natürlich insbesondere E-Bookreader oder Tablets an.

In diesem Beitrag zeige ich euch eine Möglichkeit, wie man Calibre-web selber hosten kann. 

Die Applikation zeichnet sich durch folgende Features aus:

* Filtern und Suchen nach Titeln, Autor:innen, Tags
* Büchersammlungen (Shelves) erstellen
* Editieren von Metadaten
* Bücher zu Kindle mit einem Klick senden
* Unterstützung diverse Formate: .txt, epub, .pdf usw.
* Bücher direkt im Browser lesen

## Installation des Docker Containers

Als Basis für die Installation dient bei mir ein Raspberry Pi 4 auf welchem Portainer läuft. Mit Portainer lassen sich Docker Container einfach über eine grafische Benutzeroberfläche hosten. Ich verwende das Calibre-web Docker Image von [linuxserver.io](https://hub.docker.com/r/linuxserver/calibre-web) und nutze Docker Compose. Eine YAML-Datei, in welcher alle wichtigen Parameter für den Container vordefiniert werden, dient hier als Grundlage.

Die YAML-Vorlage für Docker Compose von linuxserver.io benötigt nur wenige Anpassungen.
Ich habe lediglich die folgenden Werte angepasst:

* PUID und PGID - User-ID und Gruppen-ID unter welchen Docker auf dem Hostsystem laufen
* TZ - Ist die Zeitzone, in meinem Fall Europe/Berlin
* Volumes - Hier erfolgt die Zuordnung zum Speicherort der Konfigurationsdateien (/config) und den E-Books (/books) auf dem Hostsystem. Hierrüber wird bestimmt auf welche Ordner in eurem Dateisysten die Applikation in dem Docker-Container zugreifen darf. Der Pfad für /books auf dem Hostsystem verweist im Idealfall auf eine bestehende Calibre-Datenbank.

```yml
---
version: "2.1"
services:
  calibre-web:
    image: ghcr.io/linuxserver/calibre-web
    container_name: calibre-web
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - DOCKER_MODS=linuxserver/calibre-web:calibre
    volumes:
      - <path to data>:/config
      - <path to calibre library>:/books
    ports:
      - 8083:8083
    restart: unless-stopped
```

Nachdem ich den Container gestartet hatte, konnte ich Calibre-web nicht über den Browser erreichen. In den Logfiles tauchte folgender Fehler auf:

```
Current thread 0xb6f24460 (most recent call first):
<no Python frame>
Fatal Python error: pyinit_main: can't initialize time
Python runtime state: core initialized
PermissionError: [Errno 1] Operation not permitted
```

Die Lösung für das Problem habe ich hier auf [GitHub](https://github.com/linuxserver/docker-papermerge/issues/4) gefunden. Das Problem für den Raspberry Pi 4 ist bekannt und man muss einfach nur eine neue Version von libseccomp2 installieren.

`wget http://ftp.us.debian.org/debian/pool/main/libs/libseccomp/libseccomp2_2.5.1-1_armhf.deb`

`sudo dpkg -i libseccomp2_2.5.1-1_armhf.deb`

## Konfiguration von Calibre-web

Anschließend war Calibre im Browser über die IP Adresse meines Raspberry Pis und dem Port 8083 zu erreichen. In der Basiskonfiguration trägt man /books als Speicherort für die Calibre-Datenbank ein und klickt auf speichern.

![Calibre_1]({{ site.baseurl }}/images/calibre_1.png)

Anschließend kann man sich mit dem Username **admin** und dem Passwort **admin123** einloggen. Das Passwort kann man dann später in den Einstellungen ändern.

Nachdem ihr euch eingeloggt habt, bekommt ihr eure Büchersammlung in einer ansehnlichen Galerie präsentiert. Hier können die Bücher dann nach bestimmmten Kriterien durchsucht werden, zum Beispiel nach Autor:in. 

![Calibre_2]({{ site.baseurl }}/images/calibre_2.png)


## Bücher per Mail an den Kindle senden

Um Bücher per Mausklick als E-Mail an euren Kindle zu senden, müsst ihr zwei Dinge tun.

**Erstens**: einen E-Mailserver für den Versand konfigurieren 

Dazu unter 'Admin' auf 'Edit E-Mail Server Settings' klicken

**Zweitens**: Die E-Mailadresse eures Kindles in den Einstellungen von Calibre-web eintragen.

Dafür im Profil des Users (standardmäßig 'admin') die Kindle E-Mailadresse unter 'Send to Kindle E-Mail Address' eintragen.

Da der Kindle nur das .mobi Format unterstützt, müssen eure Ebooks in dem Format vorliegen. Generell lassen sich E-books mit Calibre-web in ein anderes Format konvertieren, allerdings ist dazu ein System mit x86-64 Chipsatz notwendig. Da im Raspberry Pi 4 ein ARM-Prozessor verbaut ist, konnte ich diese Funktion selber nicht testen. Um die Konvertierung zu aktivieren muss der Docker Container mit einem bestimmten Parameter gestartet werden, dieser kann [hier](https://github.com/linuxserver/docker-calibre-web/blob/master/README.md#parameters) nachgelesen werden.


## Bücher direkt über die Webseite hochladen

Die Uploadfunktion wird aktiviert, wenn man unter 'Admin' auf 'Edit Basic Configuration' klickt und dor unter Features 'Enable Uploads' aktiviert.

![Calibre_3]({{ site.baseurl }}/images/calibre_3.png)

Daraufhin erscheint ein Upload-Button auf der Startseite, mit welchem ihr direkt Dateien hochladen und in die Datenbank aufnehmen könnt.

## Bücher im Browser lesen

Um Bücher im Browser lesen zu können, muss für euren User die Funktion 'Allow eBook Viewer' aktiviert sein. Für den Adminuser konnte ich die Funktion in meinem Set-up nicht aktivieren. Stattdessen habe ich einfach einen neuen User erstellt und konnte für diesen dann die Funktion aktivieren. Über den Button 'Read in Browser' lässt sich dann ein Buch direkt über den Browser lesen

![Calibre_4]({{ site.baseurl }}/images/calibre_4.png)

Wenn man eine Möglichkeit sucht, die eigene Büchersammlung in einer ansehnlichen Form darzustellen und elegant zu verwalten, sollte man sich Calibre-web auf jeden Fall mal anschauen. Durch die Möglichkeit mehrere User anzulegen, kann man so auch Bücher innerhalb der Familie teilen.


**Weiterführende Links**


[GitHub Repository von Calibre-web](https://github.com/janeczku/calibre-web)  
[Projekt Gutenberg - eine digitale Bibliothek mit über 10.000 kostenlosen Büchern](https://www.projekt-gutenberg.org/)
