---
published: true
description: >-
  E-Mail-Aliase sind ein effektiver Schutz gegen Spam und Phishing. Mit speziellen Weiterleitungsdiensten lassen sich Aliase einfach erstellen und wieder abschalten.
tags:
  - privacy
  - security
header:
  image: /images/header/header_network.jpg
  teaser: /images/header/header_network.jpg
  image_description: Foto eines Briefkastens
  caption: 'Photo credit: [pexels.com](https://www.pexels.com/de-de/foto/roter-und-weisser-briefkasten-3703429/)'
title: Mehr Datenschutz und Sicherheit mit E-Mail-Weiterleitungsdiensten
---
Die eigene E-Mail-Adresse ist eines der wichtigsten persönlichen Identitätsmerkmale im Internet. Wenn man nur eine einzige E-Mail-Adresse für alle Onlinedienste verwendet, macht man es Trackern leicht dienstübergreifend verfolgt zu werden. In Falle von Datenlecks kursiert die Adresse auch schnell in diversen Datenbanken, die von Spammern missbraucht wird. Aus Gründen der Sicherheit, des Datenschutzes und der Ruhe vor Spam, sollte man also eine Verteidigungsstrategie anwenden, um die eigene E-Mail-Adresse und damit die eigene Identität zu schützen. Ich habe persönlich mit einer alten E-Mail-Adresse die leidige Erfahrung gemacht. Irgendwann habe ich darüber nur noch Spam empfangen und den Account schließlich gelöscht. 

## Was sind E-Mail-Weiterleitungsdienste  

Sogenannte E-Mail-Weiterleitungsdienste ermöglichen es Alias-Adressen zu erstellen. Ein E-Mail Alias ist eine alternative E-Mail-Adresse, welche es ermöglicht E-Mails zu empfangen ohne die eigene private Adresse offenzulegen. E-Mails, welche an die Alias-Adresse gesendet werden, werden einfach an die Hauptadresse weitergeleitet. So kann ein Spammer oder Datenhändler nicht an die persönliche Hauptadresse gelangen. Ein einfaches Beispiel: die persönliche E-Mail-Adresse lautet [maxmustermann@email.de](mailto:maxmustermann@email.de). Nun kann man den Anonymisierungsdienst SimpleLogin nutzen, um ein anonymes Alias zu erstellen. Dieses Alias leitet dann an die Hauptadresse maxmustermann@email.de. So kann man sich beispielsweise für einen Newsletter anmelden, ohne seine wirkliche Adresse offenzulegen.


![Screenshot der Aliaserstellung in SimpleLogin]({{site.baseurl}}/images/simplelogin-01.png)

Je nach verwendetem Dienst kann der lokale Part der Adresse (vor dem @) eine UUID sein, aus zufälligen Wörtern bestehen oder vom Nutzer bestimmt werden. Wenn man möchte, kann man bei vielen Weiterleitungsdiensten für jeden Account bei dem man sich anmeldet eine eigenes Alias nach dem Schema dienstname@domain.com erstellen. 

E-Mail-Weiterleitungsdienste geben den Nutzer:innen keine Mailboxen, die Speicherplatz für eure Mails zur Verfügung stellen, sondern agieren nur als Relay, welche die eingehende E-Mails unmittelbar an eure Hauptadresse weiterleiten.

 
##  Warum Aliase sinnvoll sind
 
Meiner Ansicht nach bietet die Verwendung von Aliasen mehrere Vorteile.

### Schutz vor Tracking durch Identitätsprovider

Auf [kuketz-blog.de](https://www.kuketz-blog.de/tracking-durch-identitaetsprovider/) hat [Matthias Eberl](https://social.tchncs.de/@rufposten) einen lesenswerten Blogpost über das Thema Tracking durch Identitätsprovider geschrieben. Die Identitätsprovider sind Unternehmen, deren Geschäftszweck daraus besteht Personen z.B. anhand ihrer E-Mail-Adresse zu identifizieren. Diese Daten können dann dazu genutzt werden, damit Werbeplattformen Personen webseitenübergreifend zu verfolgen. In der Regel funktioniert so: wenn man sich bei einem Shop oder einem Newsletter anmeldet, wird ein Hashwert der E-Mail-Adresse an einen Identitätsprovider übertragen. Dieser kann dann anhand des Hashwerts alle Datenpunkte derjenigen Person zusammenführen und gewinnbringend verkaufen. Für einen detaillierten Einblick in das Geschäft der Identitätsprovider empfehle ich unbedingt den Blogpost von Matthias Eberl zu lesen.

Nutzt man unterschiedliche Aliase ist diese Art des Trackings nicht mehr möglich. Jedes Alias würde einen eigene Hashwert hervorbringen, sodass sich die Daten nicht mehr anhand der E-Mail-Adresse zusammenführen lassen. 

### Effektiver Schutz vor Spam

Wenn man über ein Alias unerwünschte E-Mails / Spam erhält, lässt sich das Alias einfach deaktivieren. Der Anonymisierungsdienst leitet E-Mails an dieses Alias dann einfach nicht mehr weiter. So kann man mit einem Mausklick Spam effektiv verhindern. Würde die Hauptadresse zugespammt, müsste jeder Spammer einzeln blockiert werden, was keine effiziente Strategie gegen Spam darstellt. Sollte ein E-Mail Alias in einem Datenleak auftauchen, was früher oder später passieren wird, kann man dieses einfach deaktivieren und bei dem betroffenen Dienst ein neues Alias hinterlegen. 

### Verkauf von Nutzerdaten und Leaks erkennen

Erhält man über ein Alias, welches ausschließlich für einen Dienst verwendet wird, auf einmal Spam und andere unerwünschte E-Mails, kann man ziemlich sicher sein, dass der Dienst entweder Daten der Nutzer:innen bewusst weitergibt oder es einen Datenleak gegeben hat.

### Schutz vor Phishing

Ist eure E-Mail-Adresse erst einmal durch ein Datenleak öffentlich geworden, ist eine Angriffsfläche für Phishing entstanden. Ein aktueller Fall ist der Datenleak von über [200 Millionen E-Mail-Adressen](https://www.reuters.com/technology/twitter-hacked-200-million-user-email-addresses-leaked-researcher-says-2023-01-05/) von Twitter-Nutzer:innen. Derartige Leaks geschehen in einer erschreckenden Regelmäßigkeit. Die Daten aus solchen Leaks lassen sich für Phishingangriffe missbrauchen. Am Beispiel des Twitter-Leaks könnten Angreifer:innen sich als Support von Twitter ausgeben und so versuchen Logindaten zu stehlen oder Schadsoftware zu verteilen. Dies ist kein Problem mehr, wenn man für seinen Twitter-Account ein individuelles Alias verwendet und dieses nach dem Leak einfach abschaltet und ein neues Alias für den Account erstellt. 


## Vorstellung verschiedener Weiterleitungsdienste

Mittlerweile gibt es eine Menge Anbieter zur Weiterleitung von E-Mails, die sich teilweise nur marginal in ihren Features unterscheiden. Ich möchte exemplarisch drei Dienste vorstellen, ohne explizit eine Empfehlung für einen dieser Dienste auszusprechen. Welcher Dienst für einen der richtige ist, hängt von den eigenen Bedürfnissen ab. Habt ihr andere Empfehlungen? Schreibt mir gerne auf Mastodon, Twitter oder per E-Mail. Ggf. werde ich die Liste der Dienste auch noch erweitern.

### AnonAddy

[AnonAddy](https://anonaddy.com/) gibt es mittlerweile seit 2019. Der Dienst ist Open Source und bietet unterschiedliche Tarifstufen für unterschiedliche Bedürfnisse. Neben dem Lite und Pro Plan, gibt die Möglichkeit einen [kostenlosen Account](https://anonaddy.com/#pricing) zu erstellen. Im kostenlosen Plan ist es bereits möglich sich unbegrenzt viele Standard-Aliase anzulegen. Diese sind nach dem Schema alias@nutzername.anonaddy.com aufgebaut. Das Alias lässt sich hierbei frei wählen. Außerdem kann man 20 Shared-Domain Aliase erstellen. Dabei teilt man sich die Domain mit anderen Nutzer:innen. So lässt sich das Aliase auch nicht mehr über die Domain einer Person eindeutig zuordnen. Der kostenpflichten Lite und Pro Pläne bieten weitere Vorteile, wie etwa mehr Shared-Domain Aliase. Wenn man eine eigene Domain besitzt, kann man diese auch über AnonAddy verwenden. Weiterhin besteht die Möglichkeit AnonAddy selber auf einem eigenen Server zu hosten.

![Screenshot der Anonaddy Website]({{site.baseurl}}/images/anonaddy-01.png)

### SimpleLogin

[SimpleLogin](https://simplelogin.io/) ist ebenfalls Open Source und kommt aus Frankreich. Hier hat man die Möglichkeit entweder einen kostenlosen oder einen Premium-Account zu erstellen. In der kostenlosen Variante stehen lediglich 10 Aliase zur Verfügung. Für $30 im Jahr bekommt man die Premiumvariante, welche unbegrenzt viele Aliase ermöglicht, sowie die Einbindung einer eigenen Domain. Erwähnenswert ist hier obendrein, dass SimpleLogin 2022 von Proton (Anbieter von Proton Mail) [übernommen wurde](https://simplelogin.io/blog/simplelogin-join-proton/). Während SimpleLogin verspricht unabhängig von E-Mailprovidern zu bleiben, bieten sich hier zukünftig sicherlich Synergieffekte für Nutzer:innen von Proton Mail. 
Wie AnonAddy lässt sich SimpleLogin auch auf einem eigenen Server hosten.

![Screenshot der SimpleLogin Website]({{site.baseurl}}/images/simplelogin-02.png)

### Hide My Email

[Hide My Email](https://support.apple.com/en-us/HT210425) ist ein Dienst von Apple, welcher für die Funktion Sign in with Apple genutzt wird. Zudem ist Hide My Email in iCloud+ integriert. Im Gegensatz zu AnonAddy und SimpleLogin ist dieser Dienst also auf das Apple Ökosystem beschränkt. Wenn ihr schon ein iCloud+ Abo habt, könnt ihr Hide My Email nutzen. Der Nachteil mit der Bindung an das Ökosystem ist auch gleichzeitig ein Vorteil. Durch die enge Verzahnung ist Hide My Email sehr  zugänglich. Aliase lassen sich direkt in der Mail App von iOS, iPadOS und macOS erstellen. Weiterhin hat Apple angekündigt, dass diese zukünftig direkt im E-Mailfeld von Webseiten in Safari generiert werden können. 

![Screenshot eines iPhones mit Vorschlag E-Mail Alias in der Mail App zu erstellen.]({{site.baseurl}}/images/hidemyemail-01.png)
Quelle: [support.apple.com](https://support.apple.com/de-de/guide/iphone/iphf277f837e/ios)

### Fazit

Datenleaks, Spam, Datenhandel und Tracker, die Persönlichkeitsprofile erstellen sind eine echte Bedrohung für die Privatssphäre und die Sicherheit im Internet. E-Mail-Weiterleitungsdienste können ein essenzieller Baustein in der Verteidigungsstrategie gegen ebendiese sein. Wenn ihr  nicht ohnehin schon E-Mail-Aliase verwendet, schaut euch die vorgestellten Dienste mal genauer an und überlegt, ob ihr diese in der digitalen Selbstverteidigung einsetzen möchtet. Auch wenn ich keine Empfehlung für einzelne Dienste ausspreche, kann ich nur empfehlen, euch einen passenden Dienst herauszusuchen und diesen zu nutzen, wenn ihr dies sinnvoll findet. 