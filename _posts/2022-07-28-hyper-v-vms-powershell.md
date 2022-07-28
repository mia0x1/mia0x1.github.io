---
published: false
---
## Virtuelle Maschinen in Hyper-V mit Powershell erstellen

Der reguläre Weg Hyper-V VMs ist wohl über die GUI im Hyper-V Manager. In diesem Beitrag zeige ich, wie man virtuelle Maschinen mit Hilfe von PowerShell erstellen kann.

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.


{: .notice--info} sdfsdfsd

In modernen Windows-Umgebungen lässt Microsoft einem die Wahl Aufgaben entweder über die GUI oder mit Powershell durchzuführen. Ich habe die Erfahrung gemacht, dass Powershell oft die effizientere und bessere Methode ist. Mit Skripts und Templates lassen sich wiederkehrende Aufgaben automatisieren und man kann die dadurch eingesparte Zeit in wichtigere Projekte investieren.


In diesem Beispiel möchten wir eine VM mit Ubuntu erstellen, auf welcher dann zum Beispiel ein Ticketsystem laufen kann. Für den Anfang sollten daher erst einmal 2 CPU-Kerne und 4 GB Arbeitsspeicher ausreichend sein. Zusätzlich wird eine virtuelle Festplatte mit einer Kapazität von 100 GB erstellt und der VM zugewiesen.