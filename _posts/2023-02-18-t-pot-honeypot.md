---
published: true
description: T-Pot ist eine Plattform mit über 20 verschiedenen Honeypots. 
tags:
  - security
title: T-Pot Honeypots in Azure einrichten
---

[T-Pot](https://github.com/telekom-security/tpotce) ist eine Plattform von Telekom Security, welche über 20 verschiedene Honeypots enthält. Neben den Honeypots enthält T-Pot eine Weboberfläche mit verschiedenen Tools und Dashboards zur Datenanalyse.


![Screenshot der GitHub-Page von T-Pot]({{site.baseurl}}/images/tpot-01.png)

Im Folgenden installieren wir T-Pot in einer virtuellen Maschine in Azure.

### Systemvoraussetzungen

T-Pot benötigt 8-16 GB RAM und 128 GB freien Speicherplatz. Als OS verwenden wir Debian 11 (Bullseye).

Wir werden alle Ports der virtuelle Maschine ins öffentliche Internet exposen. Ihr solltet also entsprechende Voraussetzungen treffen und Sicherheitsbarrieren innerhalb von Azure schaffen. Ich denke für derartige Projekte sollte man auf jeden Fall eine eigene Subscription bzw. einen eigenen Tenant verwenden, welcher komplett von anderen Systemen isoliert ist.

### Installation

Als Größe der VM nehmen wir die Standard_D2s_v3 mit 2 vcpus und 8 GB RAM.
Da es sich um ein Testprojekt handelt lassen wir an dieser Stelle den Port 22 einfach offen. In Produktivumgebungen, sollte der Zugriff auf die eine oder andere Art eingeschränkt werden.

![Konfiguration der VM in Azure]({{site.baseurl}}/images/tpot-02.png)

Anschließend fügen wir eine Standard-SSD mit 128 GB Speicherplatz hinzu.

![Konfiguration der SSD in Azure]({{site.baseurl}}/images/tpot-03.png)

Die virtuelle Maschine kann jetzt erstellt werden. Wenn wir anschließend die Ressource öffnen, sehen wir oben rechts die öffentliche IP-Adresse, über welche wir uns per SSH verbinden können.

![Anzeige der IP-Adresse in Azure]({{site.baseurl}}/images/tpot-04.png)

Auf der virtuellen Maschine führen wir als erstes Systemupdates durch und installieren anschließend git und klonen dann das Repository von T-Pot.

```
sudo apt update
sudo apt upgrade -y
sudo apt install git
git clone https://github.com/telekom-security/tpotce
```

Dann wechseln wir in das Verzeichnis von T-Pot mit `cd tpotce` und starten von dort die Installation mit `sudo ./install.sh —type=user`.

Die Abfrage bestätigen wir mit Y. 

![Abfrage der T-Pot Installation, dass man bestehende Ports für bestimmte Dienste prüfen soll]({{site.baseurl}}/images/tpot-05.png)

Der T-Pot Installer wird anschließend den SSH-Port auf 64295 verlegen, weil der Port 22 für den SSH-Honeypot benötigt wird. Im Menü des Installers lassen wir die Vorauswahl auf Standard und bestätigen mit Enter.

![Screenshot des T-Pot Installers]({{site.baseurl}}/images/tpot-06.png)

Nachfolgend müssen wir Benutzername und Passwort für die Weboberfläche von T-Pot festlegen.

![Screenshot des T-Pot Installers: Festlegen des Usernamens]({{site.baseurl}}/images/tpot-07.png)

Die Installation nimmt einige Minuten in Anspruch und startet anschließend den Server automatisch neu.

![Screenshot des T-Pot Installers]({{site.baseurl}}/images/tpot-08.png)


Der T-Pot ist jetzt einsatzbereit. Damit die verschiedenen Dienste aus dem Internet erreichbar sind, müssen wir jetzt in der Netzwerksicherheitsgruppe in Azure die Verbindungen zu allen Ports zulassen. 

![Anpassung der Netzwerksicherheitsgruppe in Azure. Öffnen aller Ports für das Internet.]({{site.baseurl}}/images/tpot-09.png)


### Zugriff auf die T-Pot Weboberfläche

Anschließend erreichen wir die Weboberfläche von unserem T-Pot über https://$IP:64297 
Dort finden wir eine Vielzahl an Dashboard und Analysewerkzeugen.

![Screenshot der Weboberfläche von T-Pot]({{site.baseurl}}/images/tpot-10.png)

Im Cockpit können wir uns mit unseren SSH-Zugangsdaten anmelden und darüber den Server administrieren und Metriken auslesen. Hier gibt es sogar eine voll funktionsfähige Terminal-Emulation.

![Screenshot des Terminals im Cockpit]({{site.baseurl}}/images/tpot-11.png)

In Kibana stehen uns verschiedene vorkonfigurierte Dashboards zur Verfügung. Das T-Pot Dashboard gibt eine Vielzahl an Diagrammen aus, die Rückschlüsse auf die Angriffe auf die verschiedenen Honeypots liefern. Neben den Angriffsarten sehen wir hier auch geographische Zuordnungen der angreifenden IP-Adressen und deren Reputation. 

![Screenshot des T-Pot Dashboards]({{site.baseurl}}/images/tpot-12.png)

Im unteren Teil des Dashboard werden die Ergebnisse von Suricata angezeigt. Dies ist ein Open Source Tool zur Analyse und Erkennung von Angriffen auf Netzwerkebene. Hier sehen wir zum Beispiel genau, welche CVEs die Angreifer ausnutzen wollen.

![Screenshot der Informationen aus Suricata]({{site.baseurl}}/images/tpot-13.png)

Über das Hauptmenü der Weboberfläche können wir die Attack Map öffnen. Hier kann man Angriffe und deren geographischen Ursprung in einer interaktiven Weltkarte beobachten.

![Screenshot der Attack Map]({{site.baseurl}}/images/tpot-14.png)
