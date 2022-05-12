---
title: Der iOS Datenschutzbericht entlarvt datenhungrige Apps
description: Mit iOS 15.2 hat Apple den Datenschutzbericht eingeführt. Dies ermöglicht euch zu sehen wie datenhungrig eure Apps sind.
tags:
 - security
 - apple
 - privacy
---

Ein von mir schon länger erwartetes iOS-Feature ist der [Datenschutzbericht](https://support.apple.com/en-us/HT212958). Mit iOS 15.2 hat Apple den Bericht nun endlich ins Betriebssystem integriert. 
Der Datenschutzbericht loggt Zugriffe von Apps auf Daten und Sensoren und gibt außerdem eine Übersicht über Netzwerkzugriffe. Ihr bekommt einen detaillierten Report darüber, ob und wie oft Apps auf bestimmte Sensoren und Daten zugreifen, also zum Beispiel auf das Mikrofon, die Kamera oder eure Kontakte. Weiterhin wird protokolliert, welche Server von Apps kontaktiert werden.

## Datenschutzbericht aktivieren

Standardmäßig ist der Datenschutzbericht nicht aktiviert. Um diesen zu aktivieren geht ihr in die Einstellungen / Datenschutz / App-Datenschutzbericht.
Anschließend könnt ihr an der gleichen Stelle den Bericht abrufen. 

{:refdef: style="text-align: center;"}
![Einstellung zur Aktivierung des Datenschutzberichts.]({{ site.baseurl }}/images/datenschutzbericht_aktivieren.png)
{: refdef}


## Gesprächige Apps über Netzwerkzugriffe ertappen

Neben den Zugriffen auf die Sensoren und Daten sind die Netzwerkzugriffe das für mich interessantere Feature. Über die Netzwerkzugriffe lässt sich erkennen, welche Server bzw. Domains von Apps kontaktiert werden. Hier kann es so manche Überraschungen geben. Dies wurde in einem aktuellen Beitrag auf [Kuketz-Blog](https://www.kuketz-blog.de/ios-datenschutzbericht-daten-verbindungen-von-apps-nachvollziehen/) eindrücklich anhand der App der ARD Mediathek dargestellt. Ohne auch nur eine richtige Interaktion mit der App durchzuführen, baut diese schon Verbindungen zu Servern von Adjust GmbH (Marketinganalysen) und zu Google auf. Mike Kuketz weist darauf hin, dass es sich vermutlich um rechtswidriges Tracking handle, da hier ohne Einwilligung des Nutzers auf Informationen zugegriffen wird. 

Ich habe mir diese Erkenntnis zum Anlass genommen, um die Netzwerkzugriffe einiger weiterer Apps des öffentlichen Rundfunks und von der App des Deutschen Bundestags anzusehen. Dazu habe ich die Apps gestartet ohne irgendeine weitere Interaktion auszuführen. Daraufhin wurde im Datenschutzbericht geprüft, welche Netzwerkverbindungen aufgebaut wurden.

Als Erstes habe ich mir die App von ARTE angeschaut. Auch diese baut beim Start direkt eine Verbindung zu einem Server von Google auf.

{:refdef: style="text-align: center;"}
![Datenschutzbericht für die ARTE App.]({{ site.baseurl }}/images/datenschutzbericht_arte.png)
{: refdef}

Die 3sat-Mediathek bittet beim Start zunächst um die Zustimmung zum Tracking. Auch hier wurde bereits beim Start eine Verbindung zu Google aufgebaut.

{:refdef: style="text-align: center;"}
![Datenschutzbericht für die phoenix App.]({{ site.baseurl }}/images/datenschutzbericht_3sat.png)
{: refdef}

Weiter geht es mit der App vom Deutschen Bundestag. Hier werden direkt beim Start Verbindungen zu vier verschiedenen Google Servern aufgebaut.

{:refdef: style="text-align: center;"}
![Datenschutzbericht für die Bundestag App.]({{ site.baseurl }}/images/datenschutzbericht_bundestag.png)
{: refdef}

Die letzte App aus meinem Sample ist die Dlf Audiothek von Deutschlandradio. Auch hier findet sich wieder ein Verbindung zu einem Google Server.

{:refdef: style="text-align: center;"}
![Datenschutzbericht für die Bundestag App.]({{ site.baseurl }}/images/datenschutzbericht_dlf.png)
{: refdef}


Die rechtlichen Implikationen dieser Netzwerkverbindungen, besonders hinsichtlich der DSGVO, kann ich leider nicht beurteilen.

Was allerdings feststeht ist, dass diese Apps datenschutzfreundlicher gebaut werden können, indem zum Beispiel auf bestimmte Tracker voll und ganz verzichtet wird oder mindestens keine Daten gesendet werden, bis die Nutzer:innen ihre explizite Zustimmung dazu erteilt haben. 
Problematisch empfinde ich es, dass man Google Dienste zwar im Alltag komplett vermeiden kann, aber um Google trotzdem nicht drumherum kommt. Ich habe bewusst Apps in mein Sample aufgenommen, welche werbefrei sind und eigentlich recht datenschutzfreundlich sein sollten. Und trotzdem habe ich keine einzige App gefunden, welche zum Start keine Verbindungen zu einem Google-Server aufbaut. 

Erfreulich ist, dass durch den Datenschutzbericht solche Datenschutzprobleme an die Oberfläche getragen werden. Mein Fazit daher: Nutzt den Datenschutzbericht, analysiert die Apps, welche ihr im Alltag häufig verwendet und erzählt es gerne weiter. Dies ist ein erster guter Schritt, um gegen Tracking vorzugehen, da dieses nicht mehr im Verborgenen stattfindet, sondern für den User transparent gemacht wird.