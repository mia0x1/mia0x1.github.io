---
title: Passwörter ade - was es mit der passwortlosen Authentifizierung auf sich hat
description: Passwörter bringen viele Probleme mit, da sie leicht vergessen oder geklaut werden können. In Organisationen, aber auch bei Privatpersonen setzt sich passwortlose Authentifizierung immer weiter durch.
tags: 
 - security
---

Im Alltag schlagen wir uns täglich mit Passwörtern herum. Deswegen kennen wir alle die Probleme, welche es im Umgang mit Passwörtern geben kann. Denn man kann sie leicht vergessen oder verlieren, sei es durch Phishing, Datenleaks oder auf Grund anderer Ereignisse. Die Regelmäßigkeit von Datenleaks, bei denen fast immer auch (gehashte) Passwörter von Accounts geleaked werden, ist ziemlich alarmierend. Die zunehmende Popularität des Dienstes ['have i been pwned?'](https://haveibeenpwned.com/) zeigt auf, wie bedeutsam dieses Thema ist. Auf 'have i been pwned' können Nutzer:innen ihre E-Mailadressen und Passwörter gegen geleakte Accountdatenbanken abgleichen. Das es auch ganz ohne Passwörter gehen kann, zeigt dieser Beitrag.

Äußerst erschreckend ist, wie mit Passwörtern mitunter in der Praxis umgegangen wird. Unter den Toplisten der beliebtesten [Passwörter in Deutschland](https://hpi.de/news/jahrgaenge/2019/die-beliebtesten-deutschen-passwoerter-2019.html) finden sich Perlen wie '123456', 'password' oder 'abc123'. Haarsträubend, wenn man bedenkt, dass durch diese Passwörter oftmals persönliche und sensible Daten geschützt werden.

Hier gibt es also großen Nachholbedarf. Folgende Empfehlungen kann ich an dieser Stelle geben:

* Verwendet [sichere Passwörter](https://www.security-insider.de/was-ist-ein-sicheres-passwort-a-572229/) und nutzt bei jedem Dienst ein unterschiedliches Passwort
* Nutzt Passwortmanager wie [KeePassXC](https://keepassxc.org/) oder [Bitwarden](https://bitwarden.com/), um Passwörter zu erzeugen und zu speichern. So kann man relativ leicht bei jedem Dienst ein unterschiedliches und vor allem komplexes Passwort verwenden.
* Aktiviert die Zwei-Faktor-Authentifizierung mit einer Authenticator-App. Wenn ein Angreifer Kenntnis über euer Passwort erlangt, kann er trotzdem nicht auf den Account zugreifen, welcher immer noch durch den zweiten Faktor geschützt wird.

Diese Tipps sind für die Accountsicherheit wichtig und ich wollte sie an dieser Stelle unbedingt nochmal loswerden. Aber eigentlich sollte es in dem Beitrag ja gar nicht um Passwörter gehen, sondern um "keine Passwörter". 
Die Frage ist also an dieser Stelle: "Kann man Passwörter abschaffen und wenn ja wie?". Dazu werde ich zwei Möglichkeiten der passwortlosen Authentifizierung vorstellen, nämlich die Microsoft Authenticator App und den offenen Standard FIDO2 mit Hardware-Authenticator.



## Microsoft sagt Passwörtern den Kampf an

Microsoft hat quasi den Kampf gegen das Passwort angetreten und im hauseigenen Securityblog die ["passwortlose Zukunft"](https://www.microsoft.com/security/blog/2021/09/15/the-passwordless-future-is-here-for-your-microsoft-account/) ausgerufen. Nach eigenen Angaben nutzen bereits nahezu 100% der Mitarbeiter:innen des Unternehmens den Login ohne Passwort. Microsoft weist darauf hin, dass Passwörter zwar häufiges Angriffsziel und für uns unpraktisch sind, aber gleichzeitig den wichtigsten Security-Layer für unsere wichtigsten Accounts (E-Mail, Banking etc.) darstellen. Während Business-User schon länger die Möglichkeiten des passwortlosen Logins nutzen können, wird dieses Feature jetzt auch auf Microsoft Accounts für Privatnutzer:innen ausgerollt. Dazu bietet Microsoft verschiedene Optionen an: die hauseigene Authenticator App, Windows Hello oder auch einen Sicherheitsschlüssel, welchen ich weiter unten vorstelle. Mit der [Microsoft Authenticator App](https://docs.microsoft.com/de-de/azure/active-directory/authentication/howto-authentication-passwordless-phone) wird eine schlüsselbasierte Form der Authentifizierung ermöglicht, welche an das Gerät geknüpft wird, auf welchem die App installiert ist. Statt der Aufforderung zur Eingabe eines Passworts, bekommt man in der App die Meldung eine Zahl einzugeben, welche beim Login angezeigt wird. Diese Zahl gibt man in der App ein und stellt zusätzlich noch einen PIN oder einen biometrischen Wert bei (zum Beispiel via Face ID bei einem iPhone.)

{:refdef: style="text-align: center;"}
![Foto eines halb aufgeklappten Laptops mit beleuchteter Tastatur.]({{ site.baseurl }}/images/smartphone.jpg)
{: refdef}

Die passwortlose Authentifizierung mit dem Microsoft Authenticator eignet sich vor allem für Organisationen, welche das Microsoft Azure Active Directory nutzen, da man diese Art der passwortlosen Authentifizierung hier einfach für alle Organisationsmitglieder ausrollen und verwalten kann.

Weil Microsoft mit Windows das meist verbreitetste Computerbetriebssystem vertreibt, ist dieser Ansatz für mehr Sicherheit nur zu begrüßen. Natürlich muss man sich nicht im Microsoft Universum bewegen, um in die Vorzüge einer passwortlosen Authentifizierung zu kommen. Denn zum Glück gibt es hier offene Standards, welche man betriebssystemübergreifend nutzen kann. 


## Offener Standard: FIDO2 mit Hardware-Authenticator

Bei FIDO2 handelt es sich um ein offenes Verfahren, welches bei der Authentifizierung für Webservices Anwendung findet. Dieser Standard kann genutzt werden, um sich bei einem Webdienst komplett ohne Passwort einzuloggen. Dieser Prozess ist wesentlich sicherer als ein herkömmliches Passwort, welches gehackt oder erraten werden kann.
FIDO2 funktioniert mit asymmetrischer Kryptografie. Auf dem Server des Dienstes, bei welchem man sich authentifizieren möchte, wird ein öffentlicher Schlüssel hinterlegt. Der dazugehörige private Schlüssel ist auf dem Authenticator gespeichert. Natürlich wird für jeden Dienst ein eigenes Schlüsselpaar generiert. 

Häufig handelt es sich beim Authenticator um einen USB-Stick. Eine weit verbreitete Variante ist der [YubiKey](https://www.yubico.com/products/), welchen es in unterschiedlichen Varianten gibt. Auf dem Bild ist beispielsweise der YubiKey mit USB-C-Anschluss zu sehen.

{:refdef: style="text-align: center;"}
![Ein YubiKey neben einem Macbook und iPhone.]({{ site.baseurl }}/images/yubikey.jpg)
{: refdef}
[Quelle: Yubico Press Room images](https://brandfolder.yubico.com/yubico/press-room-images-logos)

Auf Grund der Sicherheitsarchitektur des Authenticators, lässt sich der private Schlüssel nicht auslesen. Problematisch könnte hier jedoch der Verlust des Authenticators sein. Damit dies kein Sicherheitsproblem darstellt, lässt sich der Authenticator noch über ein weiteres Sicherheitsmerkmal schützen. Dies kann eine PIN sein oder ein biometrisches Merkmal, zum Beispiel der eigene Fingerabdruck. So kann auch im Verlustfall niemand den Authenticator missbräuchlich einsetzen. Der Verlust des Authenticators ist jedoch auch ohne die Gefahr des Missbrauchs ein großes Problem, da man sich ohne diesen nicht mehr in seine Accounts einloggen kann. Daher gibt es die generelle Empfehlung bei allen Diensten, die man per FIDO2 mit Hardwarekey schützt, einen weiteren Key als Backup zu hinterlegen. Wer sich tiefergehend mit dem FIDO2-Standard beschäftigen möchte, findet umfassende eine Dokumentationen bei der [FidoAlliance](https://loginwithfido.com/).

Mit dem passwortlosen Login verschwinden viele Probleme, welche durch das traditionelle Passwort hervorgebracht wurden. Durch den Einsatz derartiger Technologien können Accounts im Vergleich zum herkömmlichen Passwort wesentlich besser geschützt werden. Jedoch haben auch die unterschiedlichen passwortlosen Systeme ihre Tücken. Man benötigt in jedem Fall eine Backuplösung, denn ein Verlust des Authenticators bedeutet die Aussperrung aus allen damit verbundenen Accounts.


