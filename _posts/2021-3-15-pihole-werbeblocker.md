---
title: Pi-hole, der Werbeblocker für das gesamte Heimnetz
description: Pihole ist ein Werbeblocker für das gesamte Netzwerk. 
tags:
 - opensource
 - selfhosting
 - linux
 - privacy 
---

In meinem ersten Blogpost stelle ich euch ein Tool vor, welches ich jedem empfehlen kann, da es effektiv Werbebanner und Tracking/Telemetrie blockiert und damit das Surfen im Web wesentlich erträglicher macht und eure Privatssphäre im Internet stärkt. Es geht um **Pi-hole**. 

## Pi-hole und DNS

Pi-hole fungiert als DNS-Server in eurem privaten Netzwerk. DNS wird oft als das Telefonbuch des Internets beschrieben. Die Abkürzung steht für **Domain Name System**. Dieses System wird in Computernetzwerken dafür benutzt, um Domains (also zum Beispiel mialikescoffee.com) in IP-Adressen aufzulösen. Domains sind Namen, die sich Menschen merken können und die IP-Adresse ist nichts anderes als eine Zahl, die einem Computer zugeordnet wird und diesen eindeutig identifiziert. Damit euer Browser den Inhalt einer Webseite von einem Server aufrufen kann, benötigt er die IP-Adresse des Servers. Menschen merken sich aber keine IP-Adressen, sondern Domains. Und hier kommt DNS als Vermittlungssystem zwischen Domainnamen und IP-Adressen ins Spiel. Um diesen Blog aufzurufen, gibt man mialikescoffee.com in den Browser ein. Nun fragt etwas vereinfacht dargestellt euer Computer bei einem DNS-Server an: "Zu welcher IP-Adresse gehört mialikescoffee.com?". Der DNS-Server antwortet dann mit der IP-Adresse des Webservers auf dem mein Blog gehostet ist.

Und was hat Pi-hole mit DNS zu tun? Wie ich schon oben geschrieben habe, nimmt Pi-hole die Rolle eines DNS-Servers in euerem privaten Netzwerk ein. 
Anstatt, dass eure DNS-Anfragen an einen DNS-Server irgendwo im Internet weitergeleitet werden, gehen diese zunächst an euer Pi-hole und hier wird entschieden wie mit den Anfragen weiter umgegangen werden soll.


## Funktionsweise des DNS-Blocking

Pi-hole verwendet Blocklisten, um zu entscheiden, ob eine DNS-Anfrage geblockt werden soll. Für Domains auf der Blockliste antwortet Pi-hole dann mit einer unbrauchbaren IP-Adresse. (0.0.0.0 für IPv4 bzw. :: für IPv6). Dieser Anfragen versinken sozusagen im Nichts. Deshalb nennt man diese Technik auch **DNS sinkhole**.
![nslookup]({{ site.baseurl }}/images/nslookup.png)

Wenn es sich um eine Domain handelt, die nicht auf der Blockliste steht, leitet Pi-hole die Anfrage an einen Upstream DNS Provider weiter, welchen ihr frei konfigurieren könnt. Dieser Upstream DNS Provider beantwortet dann euere DNS-Anfrage mit der dazugehörigen IP-Adresse. Einige Provider sind schon vorkonfiguriert und können einfach per Checkbox ausgewählt werden, zum Beispiel Cloudflare oder Quad 9. Ihr könnt aber auch selber die IP eines DNS Providers eurer Wahl eintragen.

![DNS provider]({{ site.baseurl }}/images/upstreamdns.png)


## Vorteil gegenüber anderen Werbeblockern

Meistens laufen Werbeblocker als Add-on im Browser (z.B. uBlock Origin). Diese funktionieren dann aber nur auf der Ebene einer Applikation und können nur innerhalb der Grenzen des Browsers agieren. Der Vorteil von Pi-hole ist, dass es auf DNS-Ebene für das gesamte Netzwerk Anfragen blockiert und nicht auf eine Applikation oder ein Gerät beschränkt ist. Ihr blockt damit nicht nur Werbebanner im Browser, sondern auch viele andere Sachen, z.B. das Senden von Windows-Telemetriedaten an Microsoft oder Anfragen eures geschwätzigen "Smart" TVs, wenn dieser etwa Nutzungsdaten an den Hersteller senden möchte.

Eine nette Zusammenfassung über die Anzahl aller DNS-Anfragen und die Blockrate bekommt ihr von Pi-hole im Browser geliefert.  
![Pi-hole Dashboard]({{ site.baseurl }}/images/pihole-dashboard.png)

## Achtung: Privatssphäre und Datenschutz

Obwohl Pi-hole euer Netzwerk sicherer machen und eure Privatssphäre schützen soll, gibt es hier einen Punkt zu erwähnen, der der Privatssphäre schaden kann. In der Standardkonfiguration erstellt Pi-hole ein Logfile aller aufgerufenen Domains (Query Log) mit den dazugehörigen Clients und IPs im Netzwerk. Das kann durchaus sinnvoll sein, da hierrüber Troubleshooting erleichtert wird. Wenn irgendeine Webseite nicht funktioniert, kann man im Query Log schauen und geblockte Domains freischalten. Das Problem an der Sache ist, dass das Log auf der anderen Seite ein umfassenden Überwachungswerkzeug sein könnte, zumindest aber ziemlich invasiv ist, vor allem wenn man Gäste in sein Netzwerk lässt und diese nicht darüber aufklärt.  
Daher meine Empfehlung: Deaktiviert das Query Log und anonymisiert die Logs bzw. deaktiviert sie komplett. Wenn eine Webseite oder ein Dienst nicht mehr funktioniert kann man für den Zeitraum des Troubleshootings temporär das Query Log wieder aktivieren und dann wieder deaktivieren, sobald das Problem gelöst wurde.


![Pi-hole Logo]({{ site.baseurl }}/images/pihole.png)


## Weiterführende Links

[Heise Tutorial zur Installation von Pi-hole](https://www.heise.de/tipps-tricks/Pi-Hole-auf-dem-Raspberry-Pi-einrichten-so-geht-s-4358553.html)  
[Offizelle Webseite](https://pi-hole.net/)  
[Pi-hole Subredddit](https://reddit.com/r/pihole/)  
[Malvertising (Wikipedia)](https://de.wikipedia.org/wiki/Malvertising)
