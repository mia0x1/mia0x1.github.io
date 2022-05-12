---
title: Microsoft öffnet Teams in Organisationen für Privatpersonen
description: Seit Januar ist es in Microsoft Teams möglich, dass Nutzer:innen aus Organisationen mit Privatpersonen chatten können. Dies kann ein Sicherheitsrisiko sein.
tags:
 - security
---

"Seamlessly collaborate with external customers and partners within the safety and security of your trusted Teams workspace." Mit dieser Ankündigung stellt Microsoft im [Teams Blog](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/microsoft-teams-users-can-now-chat-with-any-teams-user-outside/ba-p/3070832) ein neues Feature vor, mit welchem ab sofort alle Teams Nutzer:innen, welche Teams innerhalb ihrer Organisation benutzen, mit jedem externen, privaten Teams-Account ohne Beschränkung kommunizieren können. 

Dabei handelt es sich um solche Accounts, welche nicht Mitglieder einer Organisation sind, also einfach von Privatpersonen erstellt werden können. Bisher war dies nur mit externen Accounts möglich, die Teil einer anderen Organisation sind. 
Dieser Schritt wird von Microsoft damit begründet, dass Unternehmen generell ein Interesse daran haben, die Kommunikation nach außen aufrechtzuerhalten, damit ihr Geschäft gut läuft. Ich denke, dass es dafür nicht notwendig gewesen wäre Teams im großen Stil für alle zu öffnen. Grundsätzlich spricht aus Sicht der Security sogar einiges dagegen. Während der Nutzen in vielen Fällen fraglich sein kann, werden dadurch auf jeden Fall Sicherheitsrisiken eingeführt, welche es vorher nicht gab.

Ich möchte das Feature gar nicht generell schlecht reden. Meine Kritik bezieht sich vor allem auf die Implementierung. Statt es für alle Organisationen standardmäßig zu aktivieren, wäre ein Opt-in wesentlich sinnvoller gewesen. Nicht jede Organisation verfolgt aufmerksam die Release Notes von Teams, aber die meisten Organisationen werden sehr gut einschätzen können, ob ihre Mitglieder von privaten Teamsaccounts erreicht werden müssen und könnten dementsprechend nachjustieren.

In der Regel wird Teams als Tool innerhalb einer Organisation benutzt, um sich mit Kolleg:innen auszutauschen. Man vertraut den Personen, mit denen man tagtäglich über diesen Dienst kommuniziert. Diese Organisationsgrenze entfällt nun komplett, was auf einmal eine massive Angriffsfläche für [Phishingangriffe](https://mialikescoffee.com/social-engineering/) bietet. Angreifer:innen können sich aus diesem Vertrauen einen Vorteil verschaffen. Da Teams als mögliche Angriffsfläche bisher eher kein Thema gewesen ist, wäre es jetzt an der Zeit bei den Nutzer:innen Awareness zu schaffen. Am sichersten ist, das Feature komplett zu deaktivieren, wenn es nicht benötigt wird.

## Chats mit Privatpersonen unterbinden

Für die Deaktivierung muss man sich ins Teams Admin Center einloggen. Die entsprechende Option ist unter Users / External access zu finden.
Über eine Checkbox wird festgelegt, ob Externe, die nicht Mitglied einer Organisation sind, Kontakt zu den Mitgliedern der eigenen Organisation aufnehmen dürfen.

{:refdef: style="text-align: center;"}
![Deaktivierung des Ch.]({{ site.baseurl }}/images/teams_external.gif)
{: refdef} 

Hier finden sich auch noch weitere sicherheitsrelevante Einstellungen. Admins können an dieser Stelle zum Beispiel festlegen, ob ihre Organisation überhaupt von externen Personen anderer Organisationen erreicht werden kann oder dies nur auf bestimmte Organisationen beschränken.

