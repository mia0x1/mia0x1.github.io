---
published: false
---
[T-Pot](https://github.com/telekom-security/tpotce) ist eine Plattform von Telekom Security, welche über 20 verschiedene Honeypots enthält. Neben den Honeypots enthält T-Pot eine Weboberfläche mit verschiedenen Tools und Dashboards zur Datenanalyse.

Im Folgenden installieren wir T-Pot in einer virtuellen Maschine in Azure.

### Systemvoraussetzungen

T-Pot benötigt 8-16 GB RAM und 128 GB freien Speicherplatz. Als OS verwenden wir Debian 11 (Bullseye).

Wir werden alle Ports der virtuelle Maschine ins öffentliche Internet exposen. Ihr solltet also entsprechende Voraussetzungen treffen und Sicherheitsbarrieren innerhalb von Azure schaffen. Ich denke für derartige Projekte sollte man auf jeden Fall eine eigene Subscription bzw. einen eigenen Tenant verwenden, welcher komplett von anderen Systemen isoliert ist.

### Installation

Als Größe der VM nehmen wir die Standard_D2s_v3 mit 2 vcpus und 8 GB RAM.
Da es sich um ein Testprojekt handelt lassen wir an dieser Stelle den Port 22 einfach offen. In Produktivumgebungen, sollte der Zugriff auf die eine oder andere Art eingeschränkt werden.

Anschließend fügen wir eine Standard-SSD mit 128 GB Speicherplatz hinzu.

Die virtuelle Maschine kann jetzt erstellt werden. Wenn wir anschließend die Ressource öffnen, sehen wir oben rechts die öffentliche IP-Adresse, über welche wir uns per SSH verbinden können.

Auf der virtuellen Maschine führen wir als erstes Systemupdates durch und installieren anschließend git und klonen dann das Repository von T-Pot.

```
sudo apt update
sudo apt upgrade -y
sudo apt install git
git clone https://github.com/telekom-security/tpotce
```

Dann wechseln wir in das Verzeichnis von T-Pot mit `cd tpotce` und starten von dort die Installation mit `sudo ./install.sh —type=user`.

Den Dialog bestätigen wir mit Y. 

Der T-Pot Installer wird anschließend den SSH-Port auf 64295 verlegen, weil der Port 22 für den SSH-Honeypot benötigt wird. Im Menü des Installers lassen wir die Vorauswahl auf Standard und bestätigen mit Enter.

Nachfolgend müssen wir Benutzername und Passwort für die Weboberfläche von T-Pot festlegen.

Die Installation nimmt einige Minuten in Anspruch und startet anschließend den Server automatisch neu.

Der T-Pot ist jetzt einsatzbereit. Damit die verschiedenen Dienste aus dem Internet erreichbar sind, müssen wir jetzt in der Netzwerksicherheitsgruppe in Azure die Verbindungen zu allen Ports zulassen. 


### Zugriff auf die T-Pot Weboberfläche

Anschließend erreichen wir die Weboberfläche von unserem T-Pot über https://$IP:64297 
Dort finden wir eine Vielzahl an Dashboard und Analysewerkzeugen. 

Im Cockpit können wir uns mit unseren SSH-Zugangsdaten anmelden und dort dann den Server administrieren und Metriken auslesen. Hier gibt es sogar eine voll funktionsfähige Terminal-Emulation. 

In Kibana stehen uns vorgefertigte Dashboards zur Verfügung. Das T-Pot Dashboard gibt eine Vielzahl an Diagrammen aus, die Rückschlüsse auf die Angriffe auf die verschiedenen Honeypots liefern. Neben den Angriffsarten siehen wir hier auch geographische Zuordnungen der angreifenden IP-Adressen und deren Reputation. 

Im unteren Teil des Dashboard werden die Ergebnisse von Suricata angezeigt. Dies ist ein Open Source Tool zur Analyse und Erkennung von Angriffen auf Netzwerkebene. Hier sehen wir zum Beispiel genau, welche CVEs die Angreifer ausnutzen wollen.


In der Attack Map sieht man Angriffe Live auf einer interaktiven Weltkarte. 




















Das Offensive Pentesting Modul bei [tryhackme](https://tryhackme.com/path-action/pentesting/join) wollte ich schon vor längerer Zeit abschließen. Allerdings ist dann der Stillstand eingekehrt, als ich bei der Buffer Overflow Exploitation angekommen bin. Bis dato hatte ich mich nie richtig mit dem Thema auseinandergesetzt und die Challenges kamen mir auf den ersten Blick nicht sehr einsteigerfreundlich vor. Zufällig bin ich diese Woche wieder über das tryhackme-Modul gestolpert und habe entschieden mich in Buffer Overflows einzuarbeiten.

Dafür habe ich mich intensiv mit dem [Buffer Overflows Made Easy Video](https://www.youtube.com/watch?v=ncBblM920jw) von The Cyber Mentor beschäftigt und dieses auch als Orientierung für den Blogbeitrag verwendet. 

## Buffer Overflow

Ein Buffer ist ein Bereich des Speichers, in welchem Daten temporär zwischengelagert werden können. In diesem können zum Beispiel Informationen über den Zustand eines Programms gespeichert werden. Bei einem Buffer Overflow werden mehr Datenmengen in den Buffer geschrieben, als reserviert wurden, so dass benachbarte Speicherbereiche überschrieben werden. 

![Vereinfachte Darstellung eines Buffer Overflows]({{site.baseurl}}/images/buffer-00.png)