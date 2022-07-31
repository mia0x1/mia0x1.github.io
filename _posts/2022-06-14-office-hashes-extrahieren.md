---
title: Hashes aus passwortgeschützten Microsoft Office-Dateien extrahieren
description: Mit office2john.py lassen sich Passwort-Hashes aus passwortgeschützten Office-Dateien auslesen.
tags:
 - security
header:
 image: /images/header/header_office.jpg
 image_description: "Foto eines Schreibtischs mit Computer."
 caption: "Photo credit: [pexels.com](https://pexels.com)"
---

Bei der Vorbereitung für die Zertifizierung [eLearnSecurity Junior Penetration Tester](https://elearnsecurity.com/product/ejpt-certification/) bin ich über ein Aufgabe gestolpert, bei welcher man das Passwort eines passwortgeschützten Microsoft Word-Dokuments cracken sollte. Um dies effizient mit einem Tool wie John the Ripper oder Hashcat durchzuführen, muss zunächst der Hash des Passworts aus der Word-Datei extrahiert werden, welcher sich dann an ein Tool der Wahl übergeben lässt. Die hier genannten Tools sollten nur für Trainingszwecke an eigenen Dateien oder mit ausdrücklicher Erlaubnis, z.B. im Rahmen eines Penetrationtests, eingesetzt werden.

## office2john.py

John the Ripper enthält bereits das benötigte Skript, welches für das Extrahieren benötigt wird: **office2john.py**. Da ich zunächst nicht sicher war, ob das Skript auf der mir zur Verfügung stehenden virtuellen Maschine installiert war, habe ich es gesucht.

{:refdef: style="text-align: center;"}
![Pfad von office2john.py mit locate finden]({{ site.baseurl }}/images/locate.png)
{: refdef} 


**/usr/share/john/office2john.py** ist der standardmäßige Pfad für das Skript. Weiterhin gibt es das Skript als Download auf [GitHub](https://github.com/openwall/john/blob/bleeding-jumbo/run/office2john.py).
In meinem Fall ging es um die Datei MS_Word_Document.docx. 
Das Skript habe ich auf die passwortgeschützte .docx Datei angewendet und mir den Hash in die Datei hash.txt ausgeben lassen.

{:refdef: style="text-align: center;"}
![Den Hash der passwortgeschützten Datei in eine Datei ausgeben]({{ site.baseurl }}/images/office2john.png)
{: refdef}

Die hash.txt enthält dann den extrahierten Hash, welcher sich anschließend mit John the Ripper cracken lässt.

{:refdef: style="text-align: center;"}
![Der hash ist nun in der Datei hash.txt gespeichert)]({{ site.baseurl }}/images/hash.png)
{: refdef} 



