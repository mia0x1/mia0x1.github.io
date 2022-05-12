---
title: Was ist Social Engineering?
description: Social Engineering ist der Sammelbegriff für Methoden mit denen sicherheitsrelevante Daten durch das Ausnutzen menschlichen Verhaltens gewonnen werden.
tags: 
 - security
---

Social Engineering ist ein Sammelbegriff für Techniken, welche darauf abzielen unter der Ausnutzung menschlichen Verhaltens an sicherheitsrelevante Informationen zu gelangen und in Systeme einzudringen. In diesem Artikel wird erklärt wie Social Engineering funktioniert und verschiedene Arten von Angriffen vorgestellt werden.

Zurzeit bereite ich mich auf die Prüfung für die [Security+](https://www.comptia.org/certifications/security) Zertifizierung vor . Um das Erlernte besser verarbeiten zu können und direkt mit anderen zu teilen, dachte ich mir, dass ich einfach Artikel für den Blog schreiben kann, die sich mit dem Thema Security auseinandersetzen. Sozusagen eine eigene Artikelreihe. Der erste Artikel für meine Securityreihe beschäftigt sich mit Social Engineering. Im Folgenden erkläre ich zunächst was Social Engineering, um daran anknüpfend spezifische Methoden des Social Engineerings vorzustellen.

{:refdef: style="text-align: center;"}
![Bild mit Security Schriftzug.]({{ site.baseurl }}/images/security.jpg)
{: refdef}

Wie der Name Social Engineering schon erahnen lässt, geht es um menschliche Verhaltensweisen. Im Vordergrund stehen also nicht Schwachstellen in technischen Systemen, sondern es wird zunächst versucht das „menschliche Betriebssystem“ zu hacken. Angreifer:innen versuchen also bei Social Engineering Attacken Menschen zu manipulieren, um ihre Ziele zu erreichen, in dem sie sich etwa Zugang zu einem Firmennetzwerk verschaffen, um Informationen zu stehlen. Ganz allgemein kann man sagen, dass es einige Grundsätze gibt, welche die Basis aller Social Engineering Angriffe bilden. Diese Grundsätze beruhen auf der Ausnutzung sozialpsychologischer Mechanismen.

* Autorität: Es ist wahrscheinlich, dass Menschen jemandem gehorchen oder Gehör verschaffen, wenn diese Person für etwas Verantwortung trägt, auch wenn diese Verantwortung nur vorgegeben ist. Ein Social Engineer nutzt diesen Grundsatz, in dem er oder sie vorgibt sich in einer Rolle mit autoritären Befugnissen zu befinden, also sich zum Beispiel als Manager oder eine andere Autoritätsperson ausgibt.  

* Dringlichkeit: Wenn der Social Engineer es schafft ein Gefühl von Dringlichkeit zu erzeugen, könnten Opfer unvorsichtiger agieren. Es wird versucht Zeit zum Nachdenken zu nehmen und unüberlegte Handlungen zu forcieren. 

* Einschüchterung: Es wird ein Bedrohungsszenario aufgebaut, welches beim Opfer Gefühle der Angst oder Einschüchterung erzeugen soll.
* Vertrauen: Der Social Engineer versucht eine Vertrauensbasis zum Opfer aufzubauen, in dem der/die Angreifer:in vorgibt eine Vertrauensperson zu sein, welcher dann Glaubwürdigkeit zugeschrieben wird.  

* Knappheit: Es wird suggeriert, dass eine Ressource nicht mehr in ausreichender Menge verfügbar ist. Dies führt dazu, dass Menschen dieser Ressource einen höheren Wert zuschreiben, was sich dann ausnutzen lässt. Übrigens machen sich diesen Effekt nicht nur Hacker:innen für Social Engineering Angriffe zu Nutze, dieser wird auch gerne im Bereich des Marketings genutzt, um Menschen zu manipulieren und einen Kaufanreiz zu setzen. 

Diese Effekte werden nicht unbedingt isoliert angewendet, sondern häufig miteinander kombiniert. Ein Beispiel wäre, dass sich Angreifer:innen als Manager:in in einem Unternehmenskontext ausgeben (Autorität) und darauf pochen eine fingierte Rechnung zu begleichen, welche äußerst dringend bezahlt werden muss (Dringlichkeit). 

## Methoden des Social Engineerings
Nachdem nun grundsätzliche Prinzipien und sozialpsychologische Effekte des Social Engineerings erläutert wurden, stelle ich nun konkrete Methoden vor.
Hier würde ich persönlich zwischen technischen Methoden und persönliche Methoden unterscheiden. Technische Methoden nutzen z.B. E-Mails oder andere elektronische Kommunikation und persönliche Methoden setzen in der Regel voraus, dass Angreifer:innen sich am gleichen Ort wie Opfer befinden.

## Phishing

Die wohl bekannteste und verbreitetste Methode des Social Engineering ist das sogenannte Phishing. Die Wortschöpfung Phishing leitet sich vom englischen Fishing ab, also Angeln. Und darum geht es auch: Informationen zu angeln, in der Regel Logindaten oder andere sensible Informationen wie Kreditkartennummern. In der Regel werden Phishingangriffe via E-Mail durchgeführt, es gibt aber Unterarten des Phishing, wie Smishing (Phishing via SMS) oder Vishing (Phishing via Telefon). Phishing kann sich gegen viele Angriffsziele bzw. Personen auf einmal richten oder es werden spezifische Ziele gesucht, welche dann gezielter attackiert werden können. In dem Fall spricht man vom Spear Phishing. 
In der Regel laufen Phishingangriffe so ab, dass das Opfer eine Mail mit einem Link erhält, hinter welchem dann die Aufforderung steht Logindaten einzutippen. Das ist deswegen problematisch und regelmäßig erfolgreich, weil die gefälschten Internetseiten hinter dem Link von einer seriösen Internetseite, etwa die einer Bank oder eines sozialen Netzwerks, oft kaum zu unterscheiden sind. Generell ist also Vorsicht beim Umgang mit E-Mails geboten, insbesondere wenn diese einen Link enthalten und man zu schnellen und unüberlegten Handlungen aufgefordert wird (wir erinnern uns: Dringlichkeit, Einschüchterung etc.).  Auf menschlicher Seite ist also Sensibilisieren die wichtigste Komponente im Kampf gegen Phishing. Auf technischer Seite gibt es aber auch diverse Möglichkeiten Phishingangriffen zu begegnen, [Google Safe Browsing](https://mialikescoffee.com/safe-browsing/) ist eine davon.

![Hacker kommt aus dem Bildschirm eines Laptops und stiehlt Kreditkarte als Metapher für Social Engineering.]({{ site.baseurl }}/images/socialengineering.png)

## Pretexting

[Pretexting](https://blog.mailfence.com/de/social-engineering-pretexting/) nennt sich der Prozess, in welchem Angreifer:innen sich ein Szenario ausdenken, bei dem sie zum Beispiel eine bestimmte Person oder Institution nachahmen, auf welchem dann das Social Engineering aufbaut. Zum Beispiel könnte ein fingiertes Szenario sein, dass sich als Handwerker:in ausgegeben und so Zugang zu sensiblen Arealen erhalten wird. Eine Verteidigung wäre in diesem konkreten Fall telefonisch bei der entsprechenden Firma nachzuhaken.

## CEO-Fraud und Invoice Scam

Der CEO-Fraud ist eine Methode, welche sich zwar unter Phishing bzw. Spearphishing subsumieren lässt, aber nochmal gesondert erwähnt werden sollte. Hier geben Angreifer:innen vor eine hohe Autoritätsperson im Unternehmen zu sein (daher CEO). Mit Einschüchterung und Dringlichkeit wird dann versucht Mitarbeiter:innen dazu zu bewegen größere Geldsummen auf ein Konto zu überweisen. Gesondert hebe ich CEO-Fraud hervor, weil durch diese Methode schon große finanzielle Schäden entstanden sind. Einem Autozulieferer aus Nürnberg ist so ein Schaden von etwa 40 Millionen Euro [entstanden](https://www.faz.net/aktuell/wirtschaft/unternehmen/autozulieferer-leoni-um-millionensumme-betrogen-14390918.html). Dies ist nur ein Beispiel unter vielen.
Beim Invoice-Scam werden Rechnungen an Organisationen gesendet, in der Hoffnung, dass diese überwiesen werden. Die Rechnungen können sowohl elektronisch (per E-Mail) oder physisch (per Brief) übertragen werden. 

## Shoulder Surfing und Tailgaiting

Beim Shoulder Surfing wird versucht über die Schulter des Opfers zu schauen und so an sensible Daten zu kommen. So könnten etwa Logindaten gestohlen werden, wenn Angreifende sich unbemerkt hinter dem Opfer platzieren und Tastaturanschläge beobachten. Besteht zwischen Angreifer und Opfer ein Vertrauensverhältnis muss dies aber nicht einmal unbedingt unbemerkt geschehen. 
Beim Tailgaiting wird versucht physische Barrieren zu umgehen. Der Angreifende könnte zum Beispiel versuchen in ein geschlossenes Areal vorzudringen, welches nur für autorisierte Personen zugänglich ist. Er hängt sich an das Opfer, welches einen legitimen Zugang etwa mit einer Zugangskarte oder einem Zahlencode erlangt. Der oder die Angreifende versucht ebenfalls schnell durch die Tür zu gehen, solange diese noch geöffnet ist. Zieht man den Aspekt des Vertrauens mit ein könnte der Angreifende auch darauf hoffen, dass die Tür einfach aufgehalten wird. Vertrauen könnte auch geschaffen werden, wenn der Angreifende in eine bestimmte Rolle schlüpft. Mir fällt spontan die Verkleidung als Handwerker:in ein, welche beide Hände voller Werkzeug hat. Diese Bild lädt direkt dazu ein aus Höflichkeit die Tür aufzuhalten.
Sowohl gegen Tailgaiting als auch Shoulder Surfing ist die Sensibilisierung eine geeignete Methode, um derartige Angriffe zu unterbinden. Strikte Zugangskontrollen sind gerade beim Tailgaiting eine effektive Abwehrmethode.

## Dumpster Diving

Bei dieser Angriffsart wird der Müll einer Person bzw. Organisation nach wertvollen Informationen durchwühlt. Zunächst ist dies kein Social Engineering, aber das Social Engineering beginnt, wenn Informationen zusammengetragen wurden, welche dann im weiteren Verlauf für Social Engineering genutzt werden können, also zum Beispiel, um ein Szenario im Rahmen des Pretextings zu entwerfen. Oder die Informationen, welche aus dem Dumpster Diving zusammengetragen werden, fließen anschließend in eine Phishingkampagne ein. Um sich vor Dumpster Diving zu schützen ist es hilfreich Dokumenten vor der Entsorgung mit einem Aktenvernichter zu zerkleinern und damit unlesbar zu machen.

## Fazit

Der Artikel befasste sich im Wesentlichen mit den unterschiedlichen Arten des Social Engineerings und wie diese funktionieren. Es gibt sicherlich noch viele weitere Methoden des Social Engineering. Ich habe mich auf die konzentriert, welche wahrscheinlich am verbreitetsten sind. Lediglich am Rande erwähnt wurde, wie man sich vor Social Engineering schützen kann. Abschließend lässt sich sagen, dass Social Engineering ein großes Problem für die Sicherheit von Organisationen darstellt. Man kann noch so ausgeklügelte und perfektionierte technische Sicherheitsmaßnahmen ergreifen, am Ende bleibt der Mensch als Schwachstelle. Lässt sich diese Schwachstelle ausnutzen, bringen technische Sicherheitsmaßnahmen oft nur sehr wenig. Deshalb darf (IT-)Security sich nicht nur auf den Schutz technischer Systeme konzentrieren, sondern muss immer auch das Bewusstsein über die Gefahren des Social Engineerings vermitteln und geeignete Schutzmaßnahmen treffen.
