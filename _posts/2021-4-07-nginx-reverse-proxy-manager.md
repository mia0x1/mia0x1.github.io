---
title: Nginx Proxy Manager installieren und konfigurieren
description: Mit dem Nginx Proxy Manager lassen sich Reverse Proxies leicht verwalten. In diesem Arikel zeige ich, wie ihr das Tool installiert und konfiguriert.
tags: 
 - opensource
 - selfhosting
 - linux
---

Der [Nginx Proxy Manager](https://nginxproxymanager.com/) basiert, wie der Name schon erahnen lässt, auf dem Nginx-Webserver. Es handelt sich um ein ein Open Source Management Tool zur Konfiguration und Verwaltung von Reverse Proxies. Ohne viel Vorwissen lässt sich das Tool über eine grafische Benutzeroberfläche im Webbrowser steuern. Neben der Einrichtung von Reverse-Proxies können auch mit wenigen Mausklicks Lets Encrypt Zertifikate für eine verschlüsselte https Verbindung generiert werden.

{:refdef: style="text-align: center;"}
![Nginx Proxy Manager Logo]({{ site.baseurl }}/images/nginx_logo.png)
{: refdef}

## Was ist ein Reverse-Proxy?

Grundsätzlich ist ein Reverse-Proxy ein Verbindungsglied zwischen einem Client und einem oder mehreren Servern. Der Reverse-Proxy nimmt Anfragen von Clients an und leitet diese dann an einen Server weiter und gibt auch die Antworten des Servers an den Client zurück. 

Daraus ergeben sich mehrere Funktionen und Vorteile:

* Lastenausgleich von Clientanfragen, in dem der Reverse-Proxy Anfragen auf mehrere Server verteilt. 
* Sicherheit, denn interne Dienste und Server sind für einen Angreifer nicht direkt zu erkennen, wenn sie von außen nicht erreichbar sind und durch einen Reverse-Proxy geschützt werden
* Man möchte mehrere Dienste öffentlich erreichbar machen, hat aber nur eine öffentliche IP. Da man jeden Port nur einmal freigeben kann, stösst man sofort an eine Grenze, wenn man mehr als einen Webdienst auf den Standardports 80 und 443 erreichbar machen möchte.  
  

Das Diagramm zeigt einen exemplarischen Aufbau mit einem Reverse-Proxy, hinter welchem zwei Webdienste stehen. In diesem Beispiel befinden sich die beiden Dienste in einem privaten Netzwerk hinter einem Router, welcher nur eine öffentliche IP-Adresse hat. Die DNS-Einträge beider Domains müssen also auf die gleiche IP-Adresse zeigen. Der Reverse-Proxy kümmert sich darum, dass die Anfragen an den richtigen Server weitergeleitet werden.

{:refdef: style="text-align: center;"}
![Diagramm Reverse Proxy]({{ site.baseurl }}/images/nginx.png)
{: refdef}

## Mein Setup

Ich habe einen Raspberry Pi 4 auf welchem mehrere Dienste in Docker-Containern laufen. Bisher habe ich diese immer über die IP-Adresse und den Port im Browser aufgerufen. Zur Vereinheitlichung und Vereinfachung wollte ich diese gerne mit mehreren Domains erreichbar machen und zwar nach dem Schema: „dienstname.meinedomain.com“, also zum Beispiel rss.meinedomain.com, um meinen RSS-Feedserver zu erreichen. Außerdem erfolgten bislang alle Zugriffe unverschlüsselt via http. Stattdessen sollten alle Zugriffe verschlüsselt über https erfolgen.   
Beide Probleme lassen sich mit dem Nginx Proxy Manager lösen. 

## Installation des Nginx Proxy Manager

Die Voraussetzung ist ein Linux Server, auf welchem Docker und Docker-Compose installiert sind. Mit Docker-Compose lassen sich Docker Container einfach verwalten und erstellen auf Grundlage einer YAML-File, in welcher alle Parameter für den Container vorgegeben werden.

Alle Schritte zur Installation des Nginx Proxy Managers sind auch im [Quick-Setup](https://nginxproxymanager.com/#quick-setup) auf der offiziellen Webseite beschrieben. Die Vorlage der YAML-File sieht so aus:


```
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
  db:
    image: 'jc21/mariadb-aria:latest'
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./data/mysql:/var/lib/mysql
```

Wie ihr seht werden die Ports 80, 81 und 443 dem Nginx Proxy Manager Container zugeordnet. Die Ports 80 und 443 sind die Standardports für http und https. Über den Port 81 erreicht man das Webinterface des Nginx Proxy Managers, um diesen zu konfigurieren.

Neben dem App-Container wird ein zweiter Container mit einer MariaDB Datenbank erstellt. Hier solltet ihr auf jeden Fall das Passwort ändern. Bei den Volumes müsst ihr vor dem Doppelpunkt jeweils einen Pfad auf eurem Hostsystem angeben. Für den Nginx Proxy Manager gibt es zwei Volumes: für die Konfigurationsdateien und für die Lets Encrypt Zertifikate. Außerdem gibt es ein weiteres Volume für die MariaDB Datenbank. 

Die YAML-File für Docker Compose legt ihr auf eurem Linux Server mit dem Dateinamen `docker-compose.yml`  in einem Ordner ab, zum Beispiel unter /home/eueruser oder /opt und führt diese dann mit `docker-compose up -d` aus.

Nun kann man sich im Webinterface über die IP-Adresse des Servers und dem Port 81 anmelden. Der Nutzername ist „admin@example.com“ und das Passwort „changeme.“ Nach dem ersten Login wird man aufgefordert dies zu ändern.

![Nginx Login]({{ site.baseurl }}/images/nginx_1.png)

Unter Proxy Hosts lassen sich die Hosts konfigurieren. 

![Nginx Konfiguration]({{ site.baseurl }}/images/nginx_2.png)

Wenn man auf "Add Proxy Hosts" klickt, öffnet sich das Fenster, in welchen man alle Einstellungen für einen neuen Proxy Host vornehmen kann.

![Nginx Konfiguration]({{ site.baseurl }}/images/nginx_3.png)

Dort muss man den Domain Namen, die IP-Adresse des Hosts und den Port angeben. Mit der Funktion "Block Common Exploits" bekommt man zusätzliche Sicherheit, zum Beispiel einen Schutz vor SQL-Injections. Wichtig ist, dass ihr für den Domainnamen einen DNS-Eintrag konfiguriert habt. Ich habe dies bei mir nur lokal vorgenommen, da die Dienste nicht über das Internet erreichbar sind. Sollen eure Dienste öffentlich erreichbar sein, müsst ihr einen entsprechenden A-Record bei eurem DNS Provider für die Subdomain hinterlegen. Dies ist außerdem wichtig, wenn wir im nächsten Schritt ein Lets Encrypt Zertifikat beantragen, da ihr Lets Encrypt beweisen müsst, dass ihr tatsächlich Eigentümer:in der Domain seid, auf welche das Zertifikat ausgestellt wird.

## Lets Encrypt Zertifikat für https beantragen

Unter dem Menüpunkt SSL lassen sich Lets Encrypt Zertifikate beantragen. Dafür „Request a new SSL Certificate“ auswählen und am besten Force SSL einstellen. Damit wird verhindert, dass eine unverschlüsselte Verbindung über http / Port 80 aufgebaut werden kann.

![Nginx SSL beantragen]({{ site.baseurl }}/images/nginx_4.png)

Die Beantragung des Zertifikates sollte nur wenige Sekunden dauern. Wenn der Proxy Manager hinter einer Firewall sitzt, müsst ihr Port 80 freigeben bzw. eine entsprechende Portweiterleitung konfigurieren, um das Zertifikat zu beantragen.

## Probleme bei der Einrichtung

Während der Einrichtung hatte ich eigentlich nur ein Problem. Einige Dienste konnten über den Reverse-Proxy nicht aufgerufen werden und es wurde die Fehlermeldung "502 – Bad Gateway" ausgegeben. Dieser Fehler bedeutet, dass Nginx den Proxyhost nicht erreichen kann. In meinem Fall lag es daran, dass der entsprechende Container des Proxyhosts sich nicht in einem Netzwerk mit dem Nginx Proxy Manager befunden hat. Diese Problem konnte ich dadurch beheben, in dem ich dem Container mit dem Dienst dem nginx_default Netzwerk hinzugefügt habe. Danach hat es dann funktioniert. Ich benutze zur Containerverwaltung [Portainer](https://www.portainer.io/), daher die grafische Oberfläche.

![Nginx Netzwerk]({{ site.baseurl }}/images/nginx_5.png)


Zusammenfassend kann ich sagen, dass Nginx Proxy Manager eine sehr einfache Möglichkeit darstellt Reverse-Proxies für eure Dienste zu konfigurieren und gleichzeitig https zu ermöglichen, mit einer simplen Möglichkeit Zertifikate von Lets Encrypt zu beantragen. Es lohnt sich also in jedem Fall dies mal auszuprobieren.

**Weiterführende Links**  
 
 [Nginx Proxy Manager auf GitHub](https://github.com/jc21/nginx-proxy-manager)
