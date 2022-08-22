---
published: false
description: >-
  Mit einem internen Netzwerk lassen sich in VirtualBox virtuelle Maschinen vom
  Internet und vom Host isolieren.
tags:
  - 'windows,security'
header:
  image: /images/header/header_network.jpg
  image_description: Foto eines Switches
  caption: 'Photo credit: [pexels.com](https://www.pexels.com/de-de/@pixabay/)'
title: Interne Netzwerke in VirtualBox erstellen
---
Ein internes Netzwerk in VirtualBox ist ein Netzwerkmodus, in dem virtuelle Maschinen untereinander kommunizieren können, aber keine Kommunikation über die Grenzen des internen Netzwerks hinaus möglich ist.

Die Notwendigkeit ein virtuelles Netzwerk zu erstellen ergab sich bei mir, als ich ein Security-Lab mit VulnHub-Maschinen aufgebaut habe. [VulnHub](https://www.vulnhub.com/) stellt virtuelle Maschinen zur Verfügung, welche absichtlich Konfigurationsfehler und Sicherheitslücken enthalten. Die Sicherheitslücken stellen ein Risiko für die Netzwerksicherheit dar. Daher habe ich das Lab in einem internen Netzwerk isoliert. Für das Lab habe ich zwei virtuelle Maschinen dem interne Netzwerk hinzugefügt. Eine VM mit Kali Linux und eine VM, welche ich von VulnHub heruntergeladen habe.

## Internes Netzwerk konfigurieren

In VirtualBox muss man zunächst einen DHCP-Server erstellen. Dazu nutzt man das Tool vboxmanage, welches sich im Installationsverzeichnis von VirtualBox befindet.

```
vboxmanage dhcpserver add --network cyberlab --server-ip 10.0.0.1 --netmask 255.255.255.0 --lower-ip 10.0.0.2 --upper-ip 10.0.0.10 --enable
```

![Erstellung des DHCP-Servers mit vboxmanage]({{site.baseurl}}/images/vboxmanage.png)

* network cyberlab: Der Name des internen Netzwerkes lautet cyberlab.
* server-ip: Die IP-Adresse des DHCP-Servers
* netmask: Die Subnetzmaske des internen Netzwerks
* lower-ip: Die niedrigste IP-Adresse, die vom DHCP-Server vergeben wird
* upper-ip: Die höchste IP-Adresse, die vom DHCP-Server vergeben wird
* enable: DHCP-Server und internes Netzwerk aktivieren

Die virtuellen Maschinen müssen mit dem internen Netzwerk verbunden werden. Dazu öffnet man die Einstellungen der VM und wählt im Bereich Netzwerk "Angeschlossen an: Internes Netzwerk" und trägt darunter den Namne "Cyberlab" ein. Dies habe ich für meine Kali Linux VM und eine Vulnhub-VM ausgeführt.

![Konfiguration der Netzwerkeinstellungen der VM]({{site.baseurl}}/images/virtualbox_netzwerk.png)


Um zu verifizieren, ob alles geklappt hat, habe ich beide virtuellen Maschinen gestartet. Der Kali Linux VM wurde die IP-Adresse 10.0.0.2 zugewiesen.

![ifconfig auf der Kali Linux VM]({{site.baseurl}}/images/ifconfig.png)


Die Vulnhub VM habe ich mit einem nmap-Scan des internen Netzwerks gefunden. Dieser wurde die IP-Adresse 10.0.0.2 zugewiesen. Außer findet nmap den DHCP-Server mit der 10.0.0.1

![Host Discovery mit nmap]({{site.baseurl}}/images/nmap_vbox.png)


Als letzten Schritt habe ich die Konnektivität ins Internet getestet, welche nicht vorhanden sein dürfte. Der Ping zu Cloudflare-DNS (1.1.1.1) ist fehlgeschlagen. 

![Gescheiterter Ping aus dem internen Netzwerk.]({{site.baseurl}}/images/ping_vbox.png)


Somit wurden die Anforderungen erfolgreich umgesetzt: Die beiden virtuellen Maschinen können im internen Netzwerk von VirtualBox kommunizieren, besitzen jedoch keine Konnektivität darüber hinaus, können also weder mit meinem internen Netzwerk, noch mit dem Internet kommunizieren.
