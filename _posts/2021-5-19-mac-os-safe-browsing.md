---
title: Verwendung von Google Safe Browsing in Safari unter macOS
description: Google Safe Browsing ist ein Dienst, welcher vor schadhaften Webseiten warnt. Dieser Service ist Bestandteil von Safari und im macOS in den Systemprozess com.apple.Safari.SafeBrowsing.Service eingebaut. 
tags:
 - macos
 - privacy
 - security
---

In macOS gibt es einen Prozess mit dem Namen `com.apple.Safari.SafeBrowsing.Service`. Dieser Prozess wird von Safari aufgerufen, wenn man eine Webseite öffnen will. Er ist dafür zuständig zu prüfen, ob eine Webseite schadhafte Inhalte enthält, also um Beispiel, ob es sich um eine Phishingseite handelt. 

Wenn man eine Recherche zu diesem Prozess betreibt, sind viele Informationen, die man erhält, eher oberflächlich. In der Regel wird die Funktion des Prozesses kurz beschrieben, so wie ich dies in der Einführung zu dem Artikel auch getan habe. Aber ich wollte wissen wie der Prozess funktioniert. Was passiert im Hintergrund, wovon der User nichts mitbekommt und was für Implikationen ergeben sich daraus für die Privatsphäre?

Um ein grundsätzliches Verständnis über den Begriff des Safe Browsing zu erhalten, gehen wir ein paar Jahre zurück und gucken uns eine Idee eines der größten Apple Konkurrenten an, nämlich Google. Immer mehr Nutzer:innen des Webs fielen auf gefährliche Seiten herein. Dadurch wurden sie mit Malware infiziert oder Opfer von Phishing. Google hatte eine Idee, wie man Nutzer:innen davor schütze könnte. Es wurde eine Liste mit schadhaften Webseiten aufgestellt, welche die Basis des Safe Browsing bildete. Die erste Version des Safe Browsing war ziemlich rudimentär und stellte sich ziemlich schnell als Bedrohung für die Privatsphäre heraus. Denn der Browser sendete schlichtweg bei jedem Webseitenaufruf die komplette URL sowie die IP-Adresse an einen Google Server, um die URL dann mit der Safe Browsing Liste abzugleichen. Ziemlich creepy, oder? Deswegen wurde die Funktion zum Glück schnell überarbeitet und durch die sogenannte „Update API“ ersetzt. Diese funktioniert folgendermaßen:

* Von der URL wird der SHA-256 Hashwert gebildet, welcher dann auf 32 Bit gekürzt wird.
* Google sendet die Datenbank der gekürzten Hashwerte der URLs der schadhaften Webseiten an den Browser, so dass der Abgleich lokal stattfinden kann.  
* Gibt es eine Übereinstimmung zwischen dem gekürzten Hashwert der URL und einem Wert aus der lokalen Datenbank, sendet der Browser den gekürzten Hashwert der URL an einen Google Server, welcher mit einer vollständigen Liste aller SHA-256 Hashes antwortet, die zu dem gekürzten Hash passen könnten. Nun kann der Browser eine exakte Übereinstimmung prüfen.

Dieser neuer Ansatz bietet mehr Privatsphäre als der erste, welcher im Prinzip die gesamte Browsinghistorie an Google Server übermittelte. Statt aller URLs werden nun nur in manchen Fällen gekürzte SHA256-Hashes an Google gesendet. Jedoch ist hier anzumerken, dass auch in dieser Variante nach und nach Details über das Surfverhalten werden analysiert werden könnten. Wer mehr dazu lesen möchte, sollte sich auf jeden Fall diesen [Blogpost](https://blog.cryptographyengineering.com/2019/10/13/dear-apple-safe-browsing-might-not-be-that-safe/) von Matthew Green von der John Hopkins University durchlesen, von welchem ich im Wesentlichen die obigen Informationen bezogen habe.

Nach diesem Exkurs kommen wir jetzt zu Apple und dem oben benannten Prozess in macOS. Wer hier eine gänzlich andere Technologie als die von Google erwartet, muss an dieser Stelle enttäuscht werden. Denn tatsächlich setzt Apple einfach Google Safe Browsing in Safari unter iOS und macOS ein. Hier verhält es sich so, dass bis vor Kurzem Safari mit den Servern von Google Safe Browsing kommunizierte. Viele Safari Nutzer:innen war es sicherlich nicht bewusst, dass sie einen Google Service beim Surfen genutzt haben. Jedoch berichteten diverse Medien und [Blogs](https://thehackernews.com/2021/02/apple-will-proxy-safe-browsing-requests.html) im Februar diesen Jahres, dass Apple ab sofort die Anfragen für Google Safe Browsing unter iOS über eigene Proxyserver umleiten wird. Ich konnte allerdings keine Quelle finden, die dies auch für macOS bestätigt. Daher habe ich mir das einfach mal selber angeschaut. Mit Little Snitch kann man sehen, dass sich der Prozess mit „token.safebrowsing.apple“ verbinden möchte.

![Screenshot aus Little Snitch für Verbindung zur Safe Browsing Domain.]({{ site.baseurl }}/images/safebrowsing1.png)

Diese Domainname wird zu einer IP aufgelöst, welche Apple gehört. 

![Screenshot zur IP der Safe Browsing Anfrage, welche Apple gehört.]({{ site.baseurl }}/images/safebrowsing2.png)

Somit kann man also sagen, dass auch unter macOS der Proxyserver für Google Safe Browsing Anfragen genutzt wird.

Wer das Safe Browsing Feature testen möchte, kann dies übrigens auf der Seite [testsafebrowsing.appspot.com](https://testsafebrowsing.appspot.com) tun.

![Screenshot von testsafebrowsing.appspot.com, wo man Safe Browsing testen kann.]({{ site.baseurl }}/images/safebrowsing3.png)

Jetzt bleibt am Ende nur die Frage, ob das Safe Browsing ein sinnvolles Feature ist. Ich denke, dass Safe Browsing die Sicherheit der User verbessern kann und wohl auch wesentlich verbessert. Es ist gesichertes Wissen, dass Phishingangriffe regelmäßig sehr erfolgreich sind und es viele Menschen gibt, die Links Phishingmails anklicken. Safe Browsing kann in den Fällen, in denen die URL der Angreifer bekannt ist, vor derartigen Angriffen schützen.  
Mit erheblichem Aufwand könnte es  theoretisch möglich sein aus den Safe Browsing Anfragen Rückschlüsse auf das individuelle Surfverhalten zu ziehen. Hier sollte jede:r für sich überlegen, ob er oder sie Apple oder Google in der Hinsicht vertraut. Wenn das Vertrauen nicht vorhanden ist, lässt sich das Safe Browsing Feature in den Einstellungen von Safari deaktivieren. Standardmäßig ist es nämlich aktiviert. Ein anderer Schutz vor schadhaften Webseiten kann übrigens [Pi-hole](https://mialikescoffee.com/pihole/) mit entsprechenden Blocklisten sein. 
