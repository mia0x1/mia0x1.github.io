---
title: Oblivious DNS over HTTPS - Mehr Privatsphäre im Domain Name System
description: Mit dem Protokoll Oblivious DNS over HTTPS soll die Privatsphäre bei der Nutzung des Domain Name Systems (DNS) verbessert werden.
tags:
 - privacy
 - security
 - apple
---

Apple und Cloudflare haben sich zusammengetan, um gemeinsam ein Protokoll zu entwerfen, welches die Privatsphäre bei der Nutzung des Domain Name System (DNS) wesentlich verbessern soll. Das Protokoll trägt den etwas sperrigen Namen [Oblivious DNS over HTTPS](https://techcrunch.com/2020/12/08/cloudflare-and-apple-design-a-new-privacy-friendly-internet-protocol/), abgekürzt ODoH. Mit ODoH soll es DNS-Providern nicht mehr möglich sein Nutzer:innen von DNS-Anfragen zu identifizieren, da alle Anfragen über Proxyserver umgeleitet werden. Wie ODoH funktioniert erkläre ich in diesem Artikel.

Ich habe mich hier im Blog schon einmal mit DNS beschäftigt, und zwar beim Artikel über [Pi-hole]({{ site.baseurl }}/pihole-werbeblocker/). Oblivious DNS over HTTPS baut auf dem DNS Protokoll auf. Genauer gesagt ist ODoH eine Erweiterung von DNS over HTTPS (DoH). Mit DNS over HTTPS werden DNS-Anfragen zwischen User und DNS-Provider verschlüsselt, so dass Inhalte der Anfragen von niemanden auf dem Transportweg zwischen Nutzer:in und Provider mitgelesen oder manipuliert werden können. Es kann hier zwar kein „Man in the middle“ mitlesen, aber es ist aus Privacy Sicht bedenklich, dass ein DNS-Provider nicht nur den Inhalt der Anfragen kennt, sondern auch die IP-Adressen der User:innen, welche die Anfragen stellen. Dieses Privacy Problem soll durch ODoH behoben werden. Eine genaue Beschreibung des Protokolls, welche von den Erfinder:innen verfasst wurde, findet sich bei der [IETF](https://www.ietf.org/staging/draft-pauly-oblivious-doh-02.html). In dem Text wird nochmal explizit erwähnt, dass es sich bei ODoH um einen Entwurf handelt und nicht um einen etablierten Internetstandard.

## Was unterscheidet ODoH von DoH?

Kurzgesagt: Es wird eine Vermittlungsschicht zwischen die Nutzer:innen und die DNS-Provider gesetzt. Zu allererst wird die DNS-Anfrage verschlüsselt. Soweit kein Unterschied zu DNS over HTTPS. Jetzt kommt jedoch der entscheidende Unterschied: anstatt die Anfrage an einen DNS-Provider zu senden, wird diese über einen Proxyserver umgeleitet. Dieser Proxyserver ist die Vermittlungsebene zwischen Nutzer:innen und DNS-Providern. Der Proxyserver kennt zwar die IP-Adresse der Nutzer:innen, aber nicht den Inhalt der DNS-Anfrage, da diese ja verschlüsselt wurde. Den Schlüssel für die DNS-Anfrage kennen nur der User und der DNS-Provider. Der Proxyserver hat die Aufgabe die Anfrage an den DNS-Provider weiterzuleiten. Somit kennt der DNS-Provider nur die Identität des Proxyservers, aber nicht die der Nutzer:innen. Der DNS-Provider entschlüsselt die DNS-Anfrage und sendet die verschlüsselte Antwort zurück über den Proxyserver, welcher diese dann an den User weiterleitet.

Das Ganze nochmal stichpunktartig heruntergebrochen:

* Ein User stellt eine verschlüsselte DNS-Anfrage. Der Schlüssel ist nur dem User und dem DNS-Provider bekannt.
* Der Proxyserver erhält die verschlüsselte Anfrage, kennt somit die IP-Adresse des Users, aber nicht den Inhalt der Anfrage.
* Der DNS-Provider erhält Anfrage vom Proxyserver, kennt daher nicht die IP-Adresse des Users, aber den Inhalt der DNS-Anfrage durch die Entschlüsselung.
* Der DNS-Provider sendet verschlüsselte Antwort über den Proxyserver zurück an den User.

Damit das Protokoll wie vorgesehen funktioniert und die Privatsphäre schützen kann gibt es noch eine Bedingung: Proxyserver und DNS-Server müssen von zwei verschiedenen Anbietern kontrolliert werden, damit die Daten (Identität bzw. IP-Adresse und Inhalt der DNS-Anfrage) wirklich separiert bleiben. 

## iCloud Private Relay setzt ODoH ein

Auch wenn nicht absehbar ist, wann und ob sich ODoH als Internetstandard etabliert, wird das Protokoll schon heute eingesetzt.

Auf der diesjährigen WWDC-Keynote hat Apple das sogenannte [iCloud Private Relay](https://www.theverge.com/2021/6/10/22526881/apple-icloud-plus-privacy-subscription-services-revenue-wwdc-2021) vorgestellt. Private Relay verwendet Oblivious DNS over HTTPS und geht sogar noch einen Schritt weiter, indem nicht nur DNS-Abfragen anonymisiert werden, sondern der gesamte Traffic aus dem Safari Browser über 2 Hops geleitet wird. Auf dieser Darstellung wird das Prinzip der Anonymisierung meiner Meinung nach nochmal sehr einfach verständlich gemacht.


{:refdef: style="text-align: center;"}
![Schematische Darstellung von iCloud Private Relay]({{ site.baseurl }}/images/privaterelay.png)
{: refdef}
[Quelle: Apple WWDC 2021](https://developer.apple.com/videos/play/wwdc2021/10096/)

Privacy ist in dieser Zeit ein wichtiges Thema für viele Menschen. Daher ist es zu begrüßen, wenn Technologien und Protokolle geschaffen werden, die die Privatsphäre der Nutzer:innen im Internet stärken und den invasiven Praktiken von Trackingfirmen und Werbekonzernen etwas entgegensetzen können.


Weiterführende Links:

[Kuketz-Blog: Empfehlungsecke für DNS-Provider](https://www.kuketz-blog.de/empfehlungsecke/#dns)

