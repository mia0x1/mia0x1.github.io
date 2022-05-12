---
title: Navidrome - Mein selbstgehosteter Musikstreamingdienst
description: Navidrome ist ein Open Source Server für Musikstreaming. Dieser Beitrag zeigt wie man Navidrome als Docker Container selber hosten kann.
tags:
 - opensource
 - selfhosting
 - linux
---

Heutzutage wird Musik hauptsächlich über kommerzielle Streamingdienste wie Spotify oder Apple Music konsumiert. CDs und Vinyl verlieren immer weiter [Marktanteile](https://www.heise.de/newsticker/meldung/Marktanteil-von-Musik-Streaming-in-Deutschland-waechst-auf-46-4-Prozent-4328080.html).

Dieser Trend ist auch bei mir persönlich nicht anders. Ich streame durchschnittlich pro Tag mindestens eine Stunde Musik, wahrscheinlich sogar mehr. Das Vinyl steht demgegenüber ungehört im Regal. Aber auch meine eigene digitale Musiksammlung lagert auf irgendeiner alten Festplatte, welche bis zum Erstellen dieses Posts nicht einmal an meinen PC angeschlossen war.

Allerdings hatte ich in letzer Zeit öfter den Wunsch meine eigene Musikbibliothek mal wieder durchzusehen und vor allem durchzuhören.

Also warum nicht Vorteile des Streamings (Mobilität und deviceübergreifende Verfügbarkeit) mit der eigenen Musiksammlung kombinieren? Die Idee des selbstgehosteten Musikstreamingdienstes war geboren.

## Navidrome

Es war gerade Sonntag und ich hatte mir ohnehin vorgenommen am Blog zu arbeiten und etwas zu schreiben. Also warum nicht das Ganze mit einem praktischen Projekt verbinden?
 
Nach ein wenig Recherchearbeit bin ich auf [**Navidrome**](https://www.navidrome.org/) gestoßen. Navidrome ist ein webbasierter Server für Musikstreaming, ist Open Source und freie Software. Um Musik vom Navidrome Server zu streamen, kann man einfach die WebUI im Browser öffnen. Es gibt aber auch dedizierte Apps und Clients, mit welchen man die Musik abrufen kann.


## Installation und Vorausetzungen

Zum Zeitpunkt der Installation hatte ich bereits einen Raspberry Pi in meinem Netzwerk, auf welchem mehrere Applikationen und Dienste als Docker Container liefen. Zum Thema Docker und welche Applikationen ich sonst so hoste, werde ich in einem späteren Blogpost sicher noch ausführlicher berichten. Jedenfalls habe ich mich aufgrund dieser Basis dazu entschieden Navidrome ebenfalls als Docker Container zu hosten. Docker ist allerdings keine Voraussetzung. Ihr könnt euch Navidrome einfach [hier](https://www.navidrome.org/docs/installation/pre-built-binaries/) herunterladen und auf Windows, Linux oder MacOS installieren.

Um die Installation und Konfiguration simpel zu gestalten, verwende ich Docker Compose. Mit Docker Compose lassen sich Container mit Hilfe einer YAML file erstellen. In der YAML file werden innerhalb einer festen Struktur (markup) Variablen festgelegt. Das sind zum Beispiel Volumes, mit welchem die Docker Container auf Dateien auf dem Hostsystem zugreifen können. Im Falle von Navidrom ist das die Musikbibliothek.

In meinem Fall sieht die YAML file dann etwa so aus:


```yml
---
version: "2"
services:
  navidrome:
    image: deluan/navidrome:latest # Image, dass heruntergeladen wird
    container_name: navidrome # Name des Containers
    environment:
      - PUID=1000 # ersetzen durch eigene PUID
      - PGID=100 # ersetzen durch eigene PGID
      - TZ=Europe/Berlin # festlegen der Zeitzone
    volumes:
      - /srv/dev-disk-by-label-storage/Config/navidrome:/data  #Eigenen Pfad vor :/data einfügen
      - /srv/dev-disk-by-label-storage/fileserver/Musik:/music:ro #Eigener Pfad vor :/music:ro einsetzen     
    ports:
      - 4533:4533 # Festlegen der Ports (Port am Host:Port im Container)
    restart: unless-stopped;
```
  


  
Das Ganze habe ich mit [Portainer](https://www.portainer.io/) umgesetzt. Portainer ist eine grafische Benutzeroberfläche, mit welcher man Dockercontainer hosten kann und wo man eben auch die Compose YAML files benutzen kann.

![portainer]({{ site.baseurl }}/images/portainer.png)


Nachdem der Container gestartet war, konnte ich diesen im Browser via $hostip:4533 erreichen.


Dort sieht man zunächst diese Seite, in welcher man aufgefordert wird einen Admin User anzulegen. 

![navidrome login]({{ site.baseurl }}/images/navidrome.png)

Nachdem der User angelegt ist und Navidrom eure Musiksammlung gescannt hat, was einige Minuten dauern kann, könnt ihr es direkt benutzen. Das UI ist sehr übersichtlich gestaltet und nicht mit vielen Funktionen beladen. Ein nettes Feature ist aber, dass man seine eigenen Playlists erstellen kann, was man ja auch von Spotify und Co. kennt.

![navidrome login]({{ site.baseurl }}/images/navidrome2.png)


Neben der eigenen Web UI kann man auf den Navidrom Server mit verschiedenen Clients zugreifen, welche die Subsonic API verwenden. Eine Auflistung der Clients gibt es [hier](https://www.navidrome.org/docs/overview/). Bisher habe ich es aber nur über den Browser benutzt und kann daher zu den alternativen Clients/Apps keine Einschätzung geben-


**Weiterführende Links**  
[Navidrome auf GitHub](https://github.com/navidrome/navidrome/)






