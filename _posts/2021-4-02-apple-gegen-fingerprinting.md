---
title: Apple geht gegen Device-Fingerprinting vor
description: Mit Device Fingerprinting lässt sich Nutzerverhalten anhand von Gerätemerkmalen verfolgen. Apple möchte dies unterbinden.
tags:
 - apple
 - privacy
---

[Forbes](https://www.forbes.com/sites/johnkoetsier/2021/04/01/apple-rejecting-apps-with-fingerprinting-enabled-as-ios-14-privacy-enforcement-starts) berichtet aktuell, dass Apple begonnen hat Updates von Apps für den iOS App Store abzulehnen, welche im Konflikt mit den neuen Privacy Policys des Unternehmens stehen. Dies steht im engen Zusammenhang mit dem bevorstehenden Release von iOS 14.5, welches wegen der sogenannten 'App Tracking Transparancy' im Vorfeld insbesondere von Werbefirmen wie Facebook kritisiert wurde.    

Auf der anderen Seite wird dieses Feature von Datenschutz-Verfechter:innen begrüßt. Konkret geht es darum, dass iOS zukünftig Nutzer:innen fragen wird, ob eine App auf die IDFA (Identifier for Advertisers), also die einzigartige Werbe-ID, zugreifen darf und Nutzer:innen so über diese ID app-übergreifend tracken kann. Zusätzlich zu dieser Abfrage dürfen Apps keine anderen Trackingmethoden verwenden, um diese Regel zu umgehen. 

Im aktuellen Fall, über welchen Forbes berichtet, wurden Apps vom App Store abgelehnt, da diese Techniken zum Device-Fingerprinting verwenden. Bei dieser Technik werden aus verschiedenen Gerätemerkmalen, wie zum Beispiel Software Version, Sprache, Gerätemodell und vielen anderen, eine einmalige ID berechnet, mit welcher Nutzer:innen dann getrackt werden und Nutzungsprofile angelegt werden können, auch wenn sie den Zugriff auf die Werbe-ID verweigert hätten. Dies stellt also einen Verstoß gegen die neuen Privacy Policys dar.

Apple weist Entwickler:innen ausdrücklich darauf hin, dass es nicht zulässig ist Device-Fingerprinting anzuwenden, um Nutzer:innen eindeutig zu identifizieren.  
Siehe dazu: [https://developer.apple.com/app-store/user-privacy-and-data-use/](https://developer.apple.com/app-store/user-privacy-and-data-use/)

![apple]({{ site.baseurl }}/images/apple_1.png)

Ich denke, dass dies ein guter und wichtiger Schritt in Richtung mehr Datenschutz ist. Denn Device Fingerprinting ist etwas, dem man sich als Nutzer:in nicht wirklich entziehen kann. Forbes zieht das Fazit, dass es eine Gruppe gibt, die von diesen neuen Richtlinien profitieren und das sind die Nutzer:innen - "Good for the user!". Es bleibt also zu hoffen, dass dieses Beispiel Schule macht und auch auf anderen Systemen umgesetzt wird.

**Weiterführende Links**  

[heise.de - Web-Browser-Fingerprinting: Erkennbar auch ohne Cookie ](https://www.heise.de/newsticker/meldung/Web-Browser-Fingerprinting-Erkennbar-auch-ohne-Cookie-3597078.html)  
[netzpolitik.org - Studie: Forscher können Nutzer über Browser hinweg identifizieren](https://netzpolitik.org/2017/analyse-von-fingerprints-browser-uebertragend-moeglich/)
