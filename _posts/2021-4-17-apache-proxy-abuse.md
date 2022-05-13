---
title: Ist mein Webserver vom Apache Proxy Abuse betroffen?
description: Ein Apache Proxy Abuse liegt vor, wenn ein Apache Webserver von Angreifern als Proxy missbraucht wird. Ich habe mich gefragt, ob mein Server davon betroffen ist.
---

Zu Test- und Lernzwecken betreibe ich einen Ubuntu Server, auf welchem derzeit ein Apache2 Webserver installiert ist. Beim Lesen der Apache-Logfiles sind mir einige Ungereimtheiten aufgefallen, welche mich zunächst etwas beunruhigt haben, da ich mir diese zunächst nicht erklären konnte. Aufgrunddessen hatte ich die Vermutung, dass mein Server Opfer eines Apache Proxy Abuse ist. Was das genau ist, beschreibe ich in dem Artikel. 

Im Verzeichnis /var/logs/apache2/ sind die Logdateien von Apache gespeichert. Dort liegt unter anderem eine Datei 'access.log', in welcher die Serverzugriffe geloggt werden, also in der Regel die http-Anfragen, welche an den Server von anderen Computern gestellt werden.
 
In meinem Fall tauchten im Accesslog mehrere GET-Requests an diverse URLs auf, beispielsweise reddit.com oder amazon.com. An dem Status Code 200 lässt sich erkennen, dass die Anfragen erfolgreich waren, denn der Status Code 200 bedeutet 'success', siehe auch: [https://www.askapache.com/net/http-status-codes/#2xx_Success](https://www.askapache.com/net/http-status-codes/#2xx_Success).

![Webserver logfile]({{ site.baseurl }}/images/proxy1.png)

Da ich damit erstmal nichts anzufangen wusste, habe ich mir Hilfe in Discord und einem Matrix-Chatraum gesucht und dort das Thema mit einigen Leuten diskutiert. 
Anscheinend hatte ein Angreifer versucht den Apache-Server als Proxyserver zu missbrauchen, um seine eigene IP-Adresse zu verschleiern. 

Jetzt gab es mehrere Möglichkeiten, denn zum jetzigen Zeitpunkt war mir zumindest nicht klar, ob der Angriff erfolgreich und somit der Proxy Abuse tatsächlich möglich war. Erstens könnte es ja sein, dass der Server tatsächlich als Proxy konfiguriert ist. Eine zweite Möglichkeit ist, dass der Server so konfiguriert ist, dass keine Proxy-Requests weitergeleitet werden. Dennoch ist es im zweiten Fall so, dass Apache diese Anfragen annimmt und auch mit dem Status Code 200 beantwortet. Dies ist einfach in dem entsprechenden Standard (RFC2616) so festgelegt. Wenn Apache nun so eine Anfrage mit einer fremden URL erhält (also beispielsweise reddit.com) beantwortet er diese dann aber mit dem Inhalt auf dem lokalen Server, anstatt als Proxy die Anfrage an reddit.com weiterzuleiten.

Wer mehr darüber erfahren möchte, dem empfehle ich diesen Wiki-Beitrag zum Thema: [https://cwiki.apache.org/confluence/display/httpd/ProxyAbuse](https://cwiki.apache.org/confluence/display/httpd/ProxyAbuse)

## Analyse des Problems

Damit erstmal genug der Theorie, denn jetzt wollte ich das Problem praktisch analysieren und wissen, ob ein erfolgreicher Angriff/Proxy Abuse überhaupt möglich ist. Dabei bin ich in 2 Schritten vorgegangen:

* Erstens: Analyse der Apache Konfiguration

Ich habe mir die Konfiguration von Apache angeschaut, konnte dort aber keine Einstellungen finden, welche die Proxyfunktion aktivieren.

* Zweitens: Selber Proxy Anfragen an den Server stellen

Dazu habe ich die IP des Servers in den Proxyeinstellungen von Firefox eingetragen und versucht verschiedene Webseiten aufzurufen. Zunächst geschah in Firefox gar nichts und das access.log von Apache zeigte diese Einträge:  

![Webserver logfile]({{ site.baseurl }}/images/proxy2.png)


Zu sehen ist, dass versucht wurde eine verschlüsselte Verbindung über Port 443, also https, aufzubauen. Da ich damit nicht weitergekommen bin, habe ich in Firefox den Nur-HTTPS-Modus deaktiviert, um unverschlüsselte Abfragen über Port 80 via http zu ermöglichen. Dann das Ganze nochmal wiederholt und folgendes Ergebnis erhalten:  

Auszug aus dem Logfile:  

![Webserver logfile]({{ site.baseurl }}/images/proxy3.png)  

Ansicht in Firefox: 

![Webseiteninhalt]({{ site.baseurl }}/images/proxy4.png)


Die Anfragen an Apache waren also erfolgreich, wie man im Screenshot des Logfiles erkennen kann (Code 200). Aber anstatt, dass der Server hier als Proxy fungiert und mir den Inhalt von duckduckgo.com zeigt, wurde der lokale Inhalt des Servers übermittelt. 

Fazit: Proxy Abuse scheint nicht möglich zu sein, was daran zu erkennen ist, dass der Proxy-Request mit dem Inhalt des lokalen Servers beantwortet wird.

Trotzdem frage ich mich, warum der Server diverse solcher Anfragen erhalten hat. Ich schätze, dass es sich dabei um automatisierte Angriffsversuche handelt, mit denen versucht wird Server zu finden, welche für Proxy Abuse anfällig sind. Sollte jemand mehr über das Thema wissen, kann er oder sie sich gerne bei mir melden.
