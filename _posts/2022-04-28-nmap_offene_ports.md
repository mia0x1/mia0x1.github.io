---
title: Wie nmap offene Ports erkennt
description: In diesem Artikel erkläre ich wie der Portscanner nmap offene Ports erkennt.
tags:
 - security
header:
 image: /images/header_switch.jpg
 image_description: "Foto eines Switches von pexels.com."
---

nmap ist ein mächtiges Tool, welches vor allem für Host Discovery, OS Fingerprinting und als Portscanner zum Einsatz kommt. Ihr könnt nmap entweder in eurem eigenen Netzwerk einsetzen oder dort, wo ihr explizit die Erlaubnis erhalten habt, zum Beispiel um euer Firmennetzwerk zu auditieren oder weil ihr für einen Penetrationstest beauftragt wurdet.

Ein typischer Output eines nmap Scans sieht wie folgt aus. Die Ports 22, 53 und 80 wurden bei einem Host als offene Ports erkannt.

{:refdef: style="text-align: center;"}
![Ergebnis eines nmap Scans. Es wurden drei offene Ports für einen Host gefunden (22, 52 und 80)]({{ site.baseurl }}/images/nmap_ergebnis.png)
{: refdef} 

Doch wie erkennen nmap und andere Portscanner, ob ein Port offen oder geschlossen ist? Dazu werden Spezifikationen des TCP-Protokolls (Transmission Control Protocol) verwendet. Im TCP-Protokoll ist festgelegt, dass Client und Server eine Verbindung aushandeln, in dem sie einen Three Way Handshake durchführen. Hierfür werden drei Nachrichten ausgetauscht: SYN, SYN+ACK und ACK.

{:refdef: style="text-align: center;"}
![Diagramm eines TCP Three Way Handshakes]({{ site.baseurl }}/images/tcp_handshake.png)
{: refdef} 

Wenn der Client eine Verbindung zum Server aufbauen möchte, sendet er ein SYN-Paket (eine SYN Flag wird gesetzt, um das Paket als SYN zu markieren). Der Server antwortet daraufhin mit gesetzter SYN+ACK flag. Darauf antwortet der Client mit einem ACK und der eigentliche Datenaustausch kann beginnen. Dies ist der Fall, wenn hinter einem Port tatsächlich ein Service läuft, welcher über TCP erreichbar ist. Ist kein Service erreichbar, sendet der Server kein SYN+ACK, sondern ein RST+ACK, womit er dem Client mitteilt, dass der Port geschlossen ist.

{:refdef: style="text-align: center;"}
![Diagramm eines abgebrochenen Three Way Handshakes. Der Client sendet ein SYN-Paket und der Server antwortet mit RST+ACK.]({{ site.baseurl }}/images/tcp_handshake2.png)
{: refdef} 

Die Logik des TCP Three Way Handshakes wird von nmap und anderen Portscannern verwendet, um zu entscheiden, ob ein Port offen oder geschlossen ist. Beim ganz normalen Scan (TCP Connect) listet nmap einen Port als offen, wenn der Three Way Handshake komplett durchgeführt werden konnte. Wenn auf ein SYN ein RST+ACK empfangen wird, wird der Port als geschlossen betrachtet. Es gibt aber noch weitere Scanmethoden. Der populärste wird wohl der [SYN scan](https://nmap.org/book/synscan.html) sein. Dieser wird auch Stealth-Scan genannt, weil er keinen kompletten kompletten Three Way Handshake durchläuft, sondern nur ein SYN-Paket sendet. Antwortet der Server mit SYN+ACK, schickt der Client (nmap) ein RST und bricht damit den Handshake ab. Das SYN+ACK reicht in dem Fall, um einen Port als offen anzuerkennen. Wer sich tiefgreifender mit dem Thema TCP-Portscans beschäftigen möchte, kann sich auch noch die speziellen [Scanmethoden](https://nmap.org/book/scan-methods-null-fin-xmas-scan.html) FIN, NULL und Xmas genauer ansehen.

Die meisten populären Dienste im Internet verwenden TCP (HTTP, SMTP, SSH…). Jedoch gibt es auch viele Dienste, die UDP (User Datagram Protocol) verwenden (DNS, DHCP…). UDP ist ein verbindungsloses Protokoll und kennt daher auch keinen Three Way Handshake. Dies erschwert die Suche nach offenen Ports für Dienste, die UDP verwenden. UDP Scans sind generell langsam und mitunter wenig aussagekräftig. Aus diesen Gründen habe ich meinen Blogpost auf TCP bezogen, da TCP-Scans auch die meistverbreitete Art des Portscans sind.









