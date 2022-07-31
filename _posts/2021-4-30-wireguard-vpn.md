---
title: WireGuard VPN installieren und konfigurieren
description: WireGuard ist eine Software vom Aufbau von virtual private networks (VPN). Dieser Artikel zeigt, wie man WireGuard installiert und konfiguriert.
tags:
 - opensource
 - selfhosting
 - linux
header:
  image: /images/header/header_tunnel.jpg
  image_description: Foto eines Tunnels auf einer Straße.
  caption: "Photo credit: [pexels.com](https://www.pexels.com/de-de/@katelin-post-255918/)"
---

[WireGuard](https://www.wireguard.com/) ist eine freie Software, mit welcher sich VPNs (Virtual Private Networks) erstellen lassen. Diese erfreut sich seit einiger Zeit einer großen Beliebtheit, was nicht zuletzt daran liegt, dass WireGuard mittlerweile fester Bestandteil des Linux Kernels ist. In diesem Artikel zeige ich, wie man mit dem WireGuard VPN eine sichere Verbindung von seinem Computer oder Smartphone zu einem Ubuntu Server aufbaut. Ihr könnt Wireguard verwenden, um von unterwegs auf eure Heim- oder Firmennetz zuzugreifen.

Für den Zugriff von unterwegs auf euer Heimnetz, empfiehlt es sich, WireGuard auf einem Raspberry Pi oder einem anderen Minicomputer zu installieren. Gegebenenfalls habt ihr auch einen Router, welcher das WireGuard-Protokoll bereites unterstützt. Von unterwegs lässt sich dann ein verschlüsselter Tunnel von eurem Laptop oder Smartphone zum Raspberry Pi mit Wireguard aufbauen. So könnt ihr auf alle Dienste und Daten in eurem Netzwerk zugreifen ohne weitere Portfreigaben, neben dem Port für WireGuard, an eurer Firewall zu konfigurieren. Dies reduziert die Angriffsfläche auf euer Netzwerk immens.

Für die beispielhafte Installation in diesem Artikel habe ich folgendes verwendet:

* Ubuntu Server (20.04 LTS) 
* Einen Client, mit welchem ihr euch mit dem Server verbinden wollt. Euer OS ist dabei nebensächlich. WireGuard gibt es für Windows, Linux und macOS, aber auch für Android und iOS.

Ich gehe davon aus, dass die Konfiguration nicht als root User vorgenommen wird. Sollte man sich als root auf dem Server eingeloggt sein, kann man alle Befehle ohne das `sudo` eintippen.

Vor der Installation aktualisieren wir einmal die Paketquellen und Updaten unseren Server:

`sudo apt update && sudo apt upgrade -y`

Anschließend können wir WireGuard installieren:

`sudo apt install wireguard`


Die Grundvoraussetzung für einen funktionierenden WireGuard Server ist, dass unser System IP Pakete des WireGuard Interfaces weiterleitet. Dazu muss man die Konfigurationsdatei `/etc/sysctl.conf` anpassen, und zwar das # vor dem Eintrag net.ipv4.ip_forward=1 entfernen und speichern.

Damit die Änderungen aktiviert werden, muss man den Befehl eintippen:

`sudo sysctl -p`

Anschließend ändern wir die Berechtigungen des Ordners /etc/wireguard mit `umask 077 /etc/wireguard`.

Das Schlüsselpaar des Servers wird mit dem folgenden Befehl erstellt:

`wg genkey | sudo tee /etc/wireguard/privatekey | wg pubkey | sudo tee /etc/wireguard/publickey`

Den Private Key sollte man die Zwischenablage kopieren, da man diesen im nächsten Schritt für die Serverkonfiguration von WireGuard benötigt. Dieser lässt sich mit `sudo cat /etc/wireguard/privatekey` anzeigen.

Nun muss die Konfiguration des WireGuard Servers angelegt werden, wofür der private Schlüssel benötigt wird. Dafür die Datei `/etc/wireguard/wg0.conf` anlegen. Dazu könnt ihr den Befehl `sudo touch /etc/wireguard/wg0.conf` verwenden. In die Datei schreibt ihr mit dem Texteditor eurer Wahl folgendes:

```
[Interface]
PrivateKey = Hier den privaten Key einfügen
Address = 10.0.0.1/24
SaveConfig = true
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o ens160 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o ens160 -j MASQUERADE
ListenPort = 51820
```

![WireGuard Server Konfiguration]({{ site.baseurl }}/images/wireguard1.png)

Das ist die serverseitige Konfiguration. Jetzt müssen wir für unseren Client ebenfalls ein Schlüsselpaar anlegen und außerdem den Public Key des Clients in die Serverkonfiguration schreiben.

Schlüssel generieren:

`wg genkey | tee client_key | wg pubkey > client_pub`

Die Konfiguration des Clients sieht wie folgt aus: 

```
[Interface]
PrivateKey = Hier Private Key des Clients einfügen
Address = 10.0.0.2/32

[Peer]
PublicKey = Hier Public Key des Servers einfügen
Endpoint = Public IP des Servers:51820
AllowedIPs = 10.0.0.0/24
```

Diese Konfiguration habe ich in die WireGuard Software auf meinem Computer eingefügt:

![WireGuard Client Konfiguration]({{ site.baseurl }}/images/wireguard2.png)

In der wg.conf auf dem Server muss dann noch der Public Key des Clients eingetragen werden. Dieser wird in der Config als Peer bezeichnet.

```
[Peer]
PublicKey = Hier den Public Key des Clients eintragen
AllowedIPs = 10.0.0.2/32
```

Entsprechend der Konfiguration hat der Server im WireGuard Netzwerk die IP-Adresse 10.0.0.1 und der Client 10.0.0.2 .  Wir haben eine 24 Bit Subnetzmaske gewählt (/24 bzw. 255.255.255.0). Für die Konfiguration hätte auch jeder andere private IP-Adressbereich genutzt werden können, zum Beispiel 192.168.X.X /24 .

Nun starten wir WireGuard mit den folgenden Befehlen. Dadurch wird dann auch der Status des Services angezeigt, welcher "active" sein sollte.


```
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
sudo systemctl status wg-quick@wg0
```

Nun kann mit dem Client die Verbindung zum Server aufgebaut werden.
Wir können uns noch kurz vergewissern, ob die VPN-Verbindung wirklich funktioniert, indem wir vom Client einen Ping an den Server senden.

`ping 10.0.0.1`

![Ping an den WireGuard Server]({{ site.baseurl }}/images/wireguard3.png)

 Da wir auf den Ping eine Antwort vom Server erhalten haben, wurde die VPN-Verbindung erfolgreich hergestellt.
