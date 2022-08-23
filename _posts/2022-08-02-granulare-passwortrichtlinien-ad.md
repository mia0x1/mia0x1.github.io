---
published: true
description: >-
  Mit Fine-Grained Password Policies lassen sich granulare Passwortrichtlinien
  für unterschiedliche Nutzer:innengruppen in Windows-Domänen verwalten.
tags:
  - windows
header:
  image: /images/header/header_windows2.jpg
  teaser: /images/header/header_windows2.jpg
  image_description: Fotografie eines Backsteinhauses mit großen Fenstern und Blumenkasten.
  caption: 'Photo credit: [pexels.com](https://www.pexels.com/de-de/@mikebirdy/)'
title: Granulare Passwortrichtlinien im Active Directory erstellen
---

Vor kurzem war ich als Teilnehmer auf einem heise Security Workshop, in dem es um die Absicherung von Windows Domänen mit einem Active Directory ging. Dort wurde ein praktisches Tool zur Passwortsicherheit vorgestellt, welches überraschenderweise keiner der Teilnehmer:innen so wirklich kannte.

![password.jpg]({{site.baseurl}}/images/password.jpg)

Es handelt sich um die **Fine-Grained Password Policies** bzw. fein abgestimmte Passwortrichtlinien, im Folgenden einfach kurz FGPP genannt. Die meisten Admins werden für ihre Domäne die Passwortvorgaben mit der Gruppenrichtlinie Default Domain Policy verwalten. Diese Vorgabe gilt allerdings für die gesamte Domain für alle User. Wer sich eine granulare Passwortrichtlinien wünscht, zum Beispiel um privilegierte Accounts besser zu schützen, hat mit den FGPPs das richtige Werkzeug von Microsoft zur Verfügung.

### Einrichtung der FGPP

Die FGPP lassen sich über das Active Directory Administrative Center bzw. Active Directory-Verwaltungscenter steuern. Dieses ist Bestandteil der Remote Server Administration Tools (RSAT) und auf Domaincontrollern vorinstalliert. Während Passwortrichtlinien per GPO zu einer Organisationseinheit als Computereinstellungen umgesetzt werden, werden FGPP Sicherheitsgruppen zugewiesen. So kann eine allgemeine Passwortrichtlinie per GPO festgelegt werden und spezielle Anforderungen für privilegierte Accounts (etwa Adminaccounts) über die FGPP erstellt werden.

Im Active Directory-Verwaltungscenter gibt es den Container System, in dem sich der Passwort Settings Container befindet. Im Password Settings Container werden FGPPs konfiguriert und gespeichert.

{% include figure image_path="/images/fgpp1.png" alt="Screenshot des AD-Verwaltungscenters" caption="Den Container System öffnen, um zu den FGPPs zu gelangen." %}

{% include figure image_path="/images/fgpp2.png" alt="Screenshot des Password Settings Container Ordners" caption="Unter System findet sich der Password Settings Container." %}

Im Password Settings Container klickt man auf der rechten Fensterseite auf neu, um eine Passwortrichtlinie zu erstellen.

{% include figure image_path="/images/fgpp3.png" alt="Screenshot der FGPP-Einstellungen" caption="Die Kennworteinstellungen bieten Optionen zur Komplexität des Passworts und zu Kontosperrungsrichtlinien." %}

- Zur Besserung Übersicht gebe ich der Richtlinie den Namen entsprechend der Sicherheitsgruppe, auf welche sie angewendet wird.

- Die **Rangfolge** ist wichtig, wenn für einen Account mehrere Richtlinien gelten. Dann wird die Richtlinie mit der niedrigsten Rangfolge angewendet.

- Das **Kennwort muss Komplexitätsanforderungen** erfüllen bedeutet, dass es Zeichen aus 3 der 5 Kategorien enthalten muss: Kleinbuchstaben, Großbuchstaben, Zahlen, Sonderzeichen, Unicode-Zeichen.

- **Kennwort mit umkehrbarer Verschlüsselung speichern** nicht aktivieren, da ansonsten Passwörter entschlüsselt werden könnten
    
- Unter **Kontosperrungsrichtlinien erzwingen** kann man festlegen nach wie vielen fehlgeschlagenene Anmeldeversuchen ein Konto für Zeitraum X gesperrt werden soll.

- **Direkt anwendbar auf**: Hier wird die Sicherheitsgruppe festgelegt, für welche die Richtlinie gelten soll

Best Practices für Passwörter werden in [diesem Artikel](https://www.netwrix.com/password_best_practice.html) vorgestellt. War mein Artikel hilfreich oder habt ihr Fragen oder Anregungen dazu, schreib mir gerne bei [Twitter](https://twitter.com/jln0x1) oder eine E-Mail.
