---
title: Mit PhotoSync und PhotoPrism Fotos synchronisieren und organisieren
description: Fotos müssen nicht unbedingt in die Cloud synchronisiert werden. Wer eine Möglichkeit zum selberhosten sucht ist mit PhotoPrism bestens bedient. Dank PhotoSync werden Fotos vom Smartphone automatisch synchronisiert.
tags:
 - opensource
 - selfhosting
header:
  image: /images/header/header_fotografie.jpg
  image_description: Foto einer Person in roter Jacke, welche die Landschaft fotografiert.
  caption: "Photo credit: [pexels.com](https://www.pexels.com/de-de/@sam-forson-42814/)"
---

Apple macht es einem besonders leicht seine Devices in die Services des Ökosystems zu integrieren. Der goldene Käfig funktioniert zwar ausgesprochen gut, ein fader Beigeschmack kann dennoch bleiben. Abhängigkeit von und Kontrolle durch Apple und Verlust der Datenhoheit können die Kehrseite der Medaille sein. Dazu kommt die aktuelle [Kritik](https://www.heise.de/news/iPhone-soll-Fotos-ueberwachen-90-Organisationen-rufen-Apple-zu-Kehrtwende-auf-6169832.html) an den Überwachungsplänen des Konzerns, welcher sich sonst eher als Schützer der Privatsphäre vermarktet. Auf der Suche nach Alternativen zu iCloud-Fotos bin ich über [PhotoSync](https://www.photosync-app.com) und [PhotoPrism](https://photoprism.app/) gestolpert, welche mir auch auf Twitter und Mastodon empfohlen wurden.

iCloud-Fotos ist auf Apple Geräten standardmäßig aktiviert, alle Fotos, die man so macht, werden automatisch hochgeladen, gespeichert und geräteübergreifend synchronisiert, ohne dass man dafür etwas konfigurieren müsste. Viele haben das wahrscheinlich gar nicht auf dem Schirm und werden das erste Mal aktiv etwas von iCloud-Fotos mitbekommen, wenn Apple sich meldet und sagt, dass die 5 GB Gratisspeicher aufgebraucht sind und ob man nicht mal ein zahlungspflichtiges Abo zur Speichererweiterung abschließen möchte. Für alle, die das nicht möchten und lieber auf eine selbstgehostete Alternative setzen möchten, lohnt sich ein Blick auf PhotoPrism.

Mit PhotoPrism lassen sich Fotos organisieren und betrachten. Um die Fotos vom Smartphone zu PhotoPrism zu übertragen bietet sich PhotoSync an, welches es sowohl für Android, als auch für iOS gibt.

## PhotoSync: Fotos vom Smartphone auf ein NAS oder in die Cloud synchronisieren

Die Photosync-App kann man sich kostenlos im App Store herunterladen. Während sie in den grundsätzlichen Funktionalitäten auch kostenlos bleibt, gibt es die kostenpflichtigen Varianten PhotoSync Pro und PhotoSync Premium entweder als Abo oder zum Einmalkauf. Die Unterschiede zwischen den Versionen werden [hier](https://www.photosync-app.com/de/support/ios/answers/was-ist-der-unterschied-zwischen-photosync-pro-und-premium.html) beschrieben. Da ich meine Fotos in voller Qualität übertragen möchte, habe ich mich für die Pro-Version entschieden.

Sehr positiv: PhotoSync unterstützt diverse Übertragungsziele, wie auf dem Screenshot zu erkennen ist. Es werden eine Vielzahl an Protokollen und Cloudanbietern als Ziel unterstützt.

{:refdef: style="text-align: center;"}
![Screenshot mit möglichen Synchronisierungszielen von PhotoSync. Unterstützt werden Protokolle, wie WebDAV, FTP oder SMB. Außerdem stehen viele Cloudanbieter zur Auswahl, wie Dropbox oder Google Drive.]({{ site.baseurl }}/images/photosync_ziele.PNG)
{: refdef}

Ich habe auf meinem Raspberry Pi NAS einen SMB-Share für iPhone-Fotos angelegt, in welchen ich die Fotos mit der App synchronisiere. Die Einrichtung gestaltet sich relativ simpel. Zuerst sucht man die Übertragungsmöglichkeit aus und nimmt dann noch ein paar Einstellungen vor. Zum Beispiel kann man sich aussuchen, wie die Fotos benannt werden sollen und ob automatisch eine Unterordnerstruktur erstellt werden soll. Wichtig ist auch die Einstellung, in welcher Qualität die Fotos synchronisiert werden sollen.

Nachdem alles eingerichtet war, muss man eigentlich nur den roten Synchronisationsbutton drücken und PhotoSync beginnt damit die komplette Fotobibliothek zu synchronisieren. Das kann bei der ersten kompletten Synchronisation natürlich je nach Übertragungsgeschwindigkeit und Größe der Fotobibliothek etwas dauern.

PhotoSync gefällt mir bisher ziemlich gut. Ist der Workflow einmal konfiguriert, muss man sich eigentlich keine Gedanken mehr machen. Mit einem Klick auf den roten Synchronisationsbutton werden neue Fotos direkt synchronisiert.


## Fotos betrachten und organisieren mit PhotoPrism

PhotoPrism ist eine App zum Betrachten, Organisieren und Teilen der eigenen Fotosammlung. Diese ist Open Source und unter der AGPL-3.0 Lizenz auf [GitHub](https://github.com/photoprism/photoprism ) veröffentlicht. 

Zur Installation habe ich Docker Compose verwendet und mich an der [offiziellen Dokumentation](https://docs.photoprism.org/getting-started/raspberry-pi/) von PhotoPrism orientiert. 


Damit PhotoPrism auf dem Raspberry Pi (3 oder 4) funktioniert muss der Parameter `arm_64bit=1` in der config.txt gesetzt werden. Diese findet man unter /boot und kann in laufenden Betrieb editiert werden. Anschließend muss der Raspberry Pi einmal neu gestartet werden. 
Die Docker Compose File kann man sich mit `wget https://dl.photoprism.org/docker/arm64/docker-compose.yml` herunterladen.
Folgende Parameter sollten in der Docker Compose File auf jeden Fall angepasst werden.

* Das Admin Passwort für PhotoPrism (PHOTOPRISM_ADMIN_PASSWORD)
* Das Passwort für die Datenbank (PHOTOPRISM_DATABASE_PASSWORD, MYSQL_PASSWORD)
* Das Root Passwort für die Datenbank (MYSQL_ROOT_PASSWORD)
* Die URL, wenn ihr PhotoPrism über eine eigene Domain anstatt der IP-Adresse aufrufen wollt (PHOTOPRISM_DATABASE_PASSWORD).
* ID des Users und der Gruppe unter welchem der Container laufen soll. (user)
* Die Zuordnung der Ordner zwischen Hostsystem und Container. Das sind zum einen die Fotos und Videos (/photoprism/originals), die Einstellungen von PhotoPrism (/photoprism/storage) und die Datenbank (/var/lib/mysql)

Nachdem ich alles eingerichtet hatte, habe ich die Container (PhotoPrism und MariaDB Datenbank) mit `docker-compose up --detach` gestartet. Als Erstes muss man dann die Indexierung der Fotos starten. Dies macht man unter dem Menüpunkt Library. Die Indexierung hat bei meinem Raspberry Pi ca. 12 Stunden gedauert, wobei die CPU so ziemlich die ganze Zeit unter Volllast gelaufen ist. Je nach Hardware und Anzahl der Dateien kann die Dauer der Indexierung natürlich stark variieren. 

{:refdef: style="text-align: center;"}
![PhotoPrism App Landing Page.]({{ site.baseurl }}/images/photoprism_app.jpg)
{: refdef}

Mit einem Klick auf den Kreis unten rechts lassen sich Fotos markieren. So kann man mehrere Fotos zu einem Album zusammenfassen oder herunterladen. Außerdem kann man links unten bei einem Foto auf das Herz klicken, um dieses den Favoriten hinzuzufügen.

PhotoPrism klassifiziert Fotos und Bilder automatisch mittels Machine Learning. Dabei kommt das [TensorFlow Framework](https://de.wikipedia.org/wiki/TensorFlow) zum Einsatz.

Die Fotobibliothek kann nach Datum, Ort, Kamera, Label und weiteren Kriterien durchsucht werden. Wenn eure Fotos Geometadaten enthalten ist das Geomapping sicherlich eines der spannendsten Features von PhotoPrism. Basierend auf OpenStreetMap werden die Fotos auf einer Weltkarte eingeordnet. Bei mir sieht die Map sehr unspektakulär aus, da fast keine meiner Fotos mit Geometadaten ausgestattet sind.

{:refdef: style="text-align: center;"}
![Screenshot des GeoMappings in Photoprism.]({{ site.baseurl }}/images/photoprism_places.png)
{: refdef}

Interessant ist auch der Menüpunkt "Labels". Dort werden Fotos in Labels eingeordnet, für welche TensorFlow die Klassifizierung übernommen hat. Dies funktioniert in der Regel ganz gut, in manchen Fällen musste ich dann allerdings doch Schmunzeln. 

{:refdef: style="text-align: center;"}
![Foto einer Möve, welche von TensorFlow als Gans erkannt wurde.]({{ site.baseurl }}/images/photoprism_goose.png)
{: refdef}

Unter "Moments" erstellt PhotoPrism Momente, indem zum Beispiel ein Thema (Natur) oder ein Ereignis zusammengefasst wird (etwa Niederlande 2014).

Zusammenfassend kann ich sagen, dass ich PhotoPrism im Zusammenspiel mit PhotoSync sehr überzeugend finde. Die Einrichtung habe ich an einem Abend vorgenommen und es gab dabei keinerlei technischen Probleme. Positiv überrascht hat mich, dass PhotoPrism auch auf dem Raspberry Pi 4 relativ flüssig läuft. Wenn ihr eine Alternative zu iCloud-Fotos oder einem anderen Cloudanbieter sucht oder eure Fotos bislang nur ein einsames Dasein auf eurem Smartphone fristen, werft ruhig mal einen Blick auf PhotoPrism und PhotoSync. 
