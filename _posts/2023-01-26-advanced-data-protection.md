---
published: false
description: >-
  Mit der Advanced Data Protection führt Apple die Ende-zu-Ende Verschlüsselung
  für fast  alle Daten in iCloud ein.
tags:
  - privacy
  - security
  - apple
header:
  image: /images/header/header_key.jpg
  teaser: /images/header/header_key.jpg
  image_description: 'Foto eines Schlüssels, der an einer Wand aufgehängt wurde.'
  caption: >-
    Photo credit:
    [pexels.com](https://www.pexels.com/de-de/foto/selektive-fotografie-von-skeleton-key-hanging-217316/)
---

Mit iOS 16.2 bzw. 16.3 in Deutschland führte Apple die sogenannte Advanced Data Protection (nachfolgend einfach nur ADP genannt) für iCloud ein. Diese bietet eine Ende-zu-Ende-Verschlüsselung für Inhalte an, die in der iCloud gespeichert werden. In den vergangenen Jahren wurde kontrovers darüber diskutiert, dass etwa Backups von iPhones in der iCloud nicht Ende-zu-Ende verschlüsselt sind. Die Daten waren zwar auf den Servern von Apple im Ruhezustand verschlüsselt, jedoch kontrollierte Apple dafür die Schlüssel. 
2019 startete die EFF eine Kampagne unter dem Titel „[Fix It Already](https://fixitalready.eff.org/apple/#/)“, bei welcher sie unter anderem Apple dazu aufriefen Nutzer:innen die Verschlüsselung von Backups zu ermöglichen.

![Screenshot des EFF-Kampagnenlogos zu Fix It Already]({{site.baseurl}}/images/adp-1.png)

Ich glaube Tim Cook hatte sich in der Vergangenheit nur ein einziges Mal zu dem Thema öffentlich geäußert, nämlich in einem [Interview](https://www.spiegel.de/netzwelt/gadgets/apple-chef-tim-cook-interview-ueber-verschluesselung-recycling-und-app-store-a-1234607.html) mit Spiegel Online. Dort hielt er die Ende-zu-Ende-Verschlüsselung der Backups für eine gute Idee. 

Zugegebenermaßen habe ich nach Jahren der Debatte nicht daran geglaubt, dass Apple jemals diesen Schritt gehen wird und war von der Ankündigung positiv überrascht.

## Was genau ändert sich durch ADP?

In einer [Pressemitteilung](https://www.apple.com/newsroom/2022/12/apple-advances-user-security-with-powerful-new-data-protections/) vom 07.12.2022 erklärte Apple die Bedeutung von ADP. Dort heißt es, dass bis dato 14 Datenkategorien in iCloud standardmäßig Ende-zu-Ende verschlüsselt waren - dazu zählen Gesundheitsdaten und Passwörter in der iCloud Keychain. Mit der Einführung von ADP wurde die Verschlüsselung auf 23 Datenkategorien erhöht. Zu den neu dazugekommen Kategorien zählen unter anderem das Gerätebackup, iMessage-Nachrichten, Fotos und Notizen. Folgende Datenkategorien sind von APD ausgenommen und werden nicht verschlüsselt: iCloud Mail, Kontakte und Kalender. Begründet wird dies damit, dass diese Daten für andere Apps in einem lesbaren Format vorliegen müssen, also etwa E-Mail-Clients von Drittanbietern.

Allerdings wird von den relevanten Datenkategorien längst nicht alles Ende-zu-Ende verschlüsselt: [Zentrale Metadaten](https://www.deutschlandfunknova.de/beitrag/rueckschlag-fuer-privacy-apple-behaelt-nachschluessel-fuer-metadaten-in-der-icloud) werden mit einem von Apple kontrollierten Schlüssel nachverschlüsselt.
Die Metadaten sollen angeblich nur dafür genutzt werden, um Datenduplikate zu erkennen und diese zu deduplizieren. Wenn also 100 Nutzer:innen die gleiche Datei in iCloud hochladen, kann diese dedupliziert werden und muss nur einmal gespeichert werden. Somit ist Apple in der Lage, Speicherplatz in ihren Rechenzentren zu sparen. 

## Wie könnt ihr ADP aktivieren?

Um ADP zu nutzen, muss die Funktion von euch aktiviert werden. Standardmäßig ist diese ausgeschaltet.

Dazu müsst ihr in den iOS-Einstellungen ganz oben auf euren Namen tippen. Dort tippt ihr dann auf iCloud, um in die iCloud-Einstellungen zu kommen. In den iCloud-Einstellungen scrollt ihr runter bis zu dem Punkt „Erweiterter Datenschutz“. 

![Screenshot der ADP-Aktivierung aus den iOS Einstellungen]({{site.baseurl}}/images/adp-2.jpeg)

Beim Menüpunkt "Erweiterter Datenschutz" könnt ihr ADP aktivieren. Als Voraussetzung gilt, dass alle Geräte, die mit eurer Apple-ID verknüpft sind, die zum heutigen Zeitpunkt aktuellste Version des Betriebssystem installiert haben. Außerdem müsst ihr vor der Aktivierung entweder einen Wiederherstellungskontakt auswählen oder einen [Wiederherstellungsschlüssel](https://support.apple.com/de-de/HT208072) erstellen. Der Schlüssel wird automatisch generiert und besteht aus 28 Zeichen. Dieser sollte aufgeschrieben und an einem sicheren Ort verwahrt werden. Die Wiederherstellungsmethoden sind notwendig, um Daten aus der iCloud wiederherzustellen, falls ihr euer Passwort vergessen solltet.


















