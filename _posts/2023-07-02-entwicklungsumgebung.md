---
published: true
description: Um einen ordentlichen Workflow bei der Erstellung von Blogposts und für Verändungen am Blog zu schaffen, habe ich eine Entwicklungsumgebung auf einer Ubuntu VM aufgesetzt.
tags:
  - selfhosting
  - linux
title: Entwicklungsumgebung zum Bloggen
---

Ich habe kürzlich mit dem Gedanken gespielt, meinen Blog von Jekyll zu Wordpress zu migrieren. Allerdings bin ich schnell zu dem Fazit gekommen, dass ich gar kein Problem mit Jekyll habe, sondern mir ein ordentlicher Workflow für die Arbeit am Blog fehlt. Ich bevorzuge die statischen, schlanken Seiten, die von Jekyll generiert werden und möchte gar kein CMS-Schwergewicht a la Wordpress einsetzen. Also habe ich die Idee der Migration schnell wieder verworfen und mir stattdessen eine Entwicklungsumgebung für Jekyll geschaffen, die die Arbeit am Blog für mich vereinfacht.

Bis dato habe ich Blogposts in der Regel in Sublime oder mit [prose.io](https://prose.io/) für GitHub geschrieben und anschließend Korrektur gelesen. Die Posts habe ich dann zu GitHub hochgeladen, ohne mir vorher nochmal anzusehen wie diese fertig gerendert aussehen. Diesen Prozess empfand ich eher als störend, was sich dann auch auf die Motivation zum Schreiben auswirkte. Außerdem fehlte mir so komplett die Möglichkeit Änderungen am Layout und der Struktur des Blogs außerhalb der produktiven Umgebung vorzunehmen.
Daher bin ich froh mich jetzt von dem bisherigen "Prozess" zu verabschieden.

Und um dies gebührend zu feiern, stelle ich in diesem Post meine Entwicklungsumgebung vor, in welcher ich ab sofort Änderungen am Blog teste und neue Artikel schreiben werde.

**tl;dr** Ich habe eine VM mit Ubuntu Server auf meinem Mac mit Parallels erstellt und dort Jekyll installiert. Die Entwicklungs- und Schreibarbeit führe ich direkt auf dem Server durch, indem ich Visual Studio Code mit SSH Erweiterung nutze.

## Ubuntu VM als Basis

Als Basis für die Entwicklungsumgebung dient ein [Ubuntu 22.04.2 LTS for ARM](https://ubuntu.com/download/server/arm), weil ich diese auf einem Mac mit M1 Chip betreibe.

## Installation von Jekyll

Auf der Ubuntu VM habe ich dann Jekyll installiert. 
Hierzu gibt es ein hilfreiches Tutorial bei [Digitalocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jekyll-development-site-on-ubuntu-20-04).

Wenn ihr eure Jekyll Seite auch auf GitHub Pages hostet, gibt es bei GitHub ein entsprechendes [Tutorial](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll?platform=linux). In diesem Tutorial wird auch darauf hingewiesen, dass man eine Fehlermeldung bekommt, wenn man Jekyll startet und Ruby 3.0 oder neuer installiert hat. In diesem Fall muss webrick mit `bundle add webrick`nachinstalliert werden. 

Um den Jekyll Blog in der Entwicklungsumgebung zu starten, habe ich noch schnell ein kurzes Skript geschrieben, wodurch der Blog über die IP-Adresse der Ubuntu VM erreichbar ist. Da diese zum Zeitpunkt der Erstellung der VM nicht statisch war, verwendet das Skript einfach die aktuelle IP-Adresse, über welche sich die Jekyll Website dann im Browser aufrufen lässt.

```bash
blog=/path/to/blog
serverip=$(ip addr show | grep -w inet | grep -v 127.0.0.1 | awk '{print $2}' | cut -d "/" -f 1)

cd $blog
bundle exec jekyll serve --host=$serverip
```

## GitHub konfigurieren

Nach der Installation von Jekyll habe ich das Repository meines Blogs von GitHub geklont.

Um Änderungen von der Entwicklungsumgebung auf GitHub zu pushen, habe ich einen neuen SSH-Key angelegt. Der Public Key muss auf GitHub in den [Einstellungen](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) hinterlegt werden. 
Ob die SSH-Verbindung funktioniert, lässt sich mit `git -T git@github.com` überprüfen. Wenn alles korrekt ist, bekommt man folgende Rückmeldung:

`Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.`  

Für die Verbindung des lokalen Repositories mit GitHub muss dann noch im Verzeichnis des lokalen Repos dieser Befehl ausgeführt werden.

```
git remote set-url origin git@github.com:username/your-repository.git
```

Damit ich lokal an Entwürfen arbeiten kann, ohne dass diese zu GitHub gepusht werden habe ich folgende Ergänzung in der .gitignore vorgenommen:

`_posts/*entwurf*`

Solange der Dateiname den String 'entwurf' enthält, wird die Datei von git ignoriert und nicht hochgeladen. Wenn ein Artikel fertig ist, muss ich einfach nur den Dateinamen anpassen und kann diesen dann zu GitHub pushen, woraufhin dieser Post dann im Blog erscheint.

## Visual Studio Code via SSH

Um Beiträge zu schreiben und den Blog weiterzuentwickeln, nutze ich Visual Studio Code. 
Mit der [Remote SSH Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) kann man sich per SSH auf die Entwicklungsumgebung verbinden und direkt auf der Entwicklungsumgebung arbeiten.

![Screenshot der Entwicklungsumgebung mit VS Code]({{site.baseurl}}/images/devenv.png)

## Ausblick

Grundsätzlich fühle ich mich jetzt schon viel besser fürs Bloggen aufgestellt.
Als sinnvollen nächsten Schritt sehe ich die Integration von LanguagueTool in VS Code, wodurch eine direkte Überprüfung von Rechtschreibung und Grammatik ermöglicht wird. Dazu habe ich eine Installationsanleitung bei [gnulinux.ch](https://gnulinux.ch/languagetool-in-vs-code) gefunden, welche ich bei Gelegenheit gerne übernehmen möchte.
