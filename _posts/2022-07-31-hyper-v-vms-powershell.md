---
published: true
description: >-
  Mit Powershell lassen sich virtuelle Maschinen für Hyper-V erstellen und
  konfigurieren.
tags:
  - windows
header:
  image: /images/header/header_windows.jpg
  teaser: /images/header/header_windows.jpg
  image_description: Fotografie eines Wohnhauses aus einem Fenster heraus.
  caption: 'Photo credit: [pexels.com](https://pexels.com)'
title: Virtuelle Maschinen in Hyper-V mit Powershell erstellen
---
Der herkömmliche Weg Hyper-V VMs zu installieren ist wohl mittels der GUI im Hyper-V Manager. In diesem Beitrag zeige ich, wie man virtuelle Maschinen in Hyper-V mithilfe von Powershell erstellen kann.

In modernen Windows-Umgebungen lässt Microsoft einem die Wahl Aufgaben entweder über die GUI oder mit Powershell durchzuführen. Ich habe die Erfahrung gemacht, dass Powershell oft die effizientere und bessere Methode ist. Mit Skripts und Templates lassen sich wiederkehrende Aufgaben automatisieren und man kann die dadurch eingesparte Zeit in wichtigere Projekte investieren.

In diesem Beispiel erstelle ich eine VM mit Windows Server 2022. Dem Server werden 2 virtuelle CPU-Kerne und 4 GB Arbeitsspeicher zugewiesen. Zusätzlich wird eine virtuelle Festplatte mit einer Kapazität von 100 GB erstellt und der VM zugewiesen.

## Cmdlets und Parameter

* New-VM: Cmdlet zur Erstellung einer neuen VM
* Name: Name der VM
* -MemoryStartUpBytes: Größe des Arbeitsspeichers
* -NewVHDPath: Pfad der virtuellen Festplatte auf dem Hyper-V Host
* -Path: Pfad der VM auf dem Hyper-V Host
* -NewVHDSizeBytes: Größe der VHD
* -[Generation](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v): Generation 1 oder 2 der VM (Gen 2 unterstützt UEFI / Secure Boot)

* AddVMDvdDrive: Fügt ein DVD-Laufwerk zu einer VM hinzu
* -VMName: Name der VM, zu welcher das DVD-Laufwerk hinzugefügt werden soll
* -Path: Pfad der ISO-Datei auf dem Host

* Set-VMProcessor: Virtuelle CPUs einer VM konfigurieren
* -Count: Anzahl der CPUs


Zuerst muss Powershell als Administrator ausgeführt werden. Nun lässt sich mit dem New-VM cmdlet eine neue virtuelle Maschine erstellen.
```
New-VM -name "ws2022" -MemoryStartupBytes 4GB -NewVHDPath "C:\Hyper-V\Virtual Hard Disks\ws2022.vhdx" -Path "C:\Hyper-V\Virtual Machines\" -NewVHDSizeBytes 100GB -Generation 2
```
![Powershell-Syntax zur Erstellung der VM]({{site.baseurl}}/images/new-vm.png)

Nachdem der Befehl ausgegeben wurde, erscheint die VM im Hyper-V Manager.

![Screenshot des Hyper-V Managers mit der neuen VM]({{site.baseurl}}/images/hyper-v-manager.png)

Jetzt müssen die CPUs konfiguriert werden.

```
Set-VMProcessor ws2022 -count 2
```

Bevor die VM gestartet werden kann, wird noch das DVD-Laufwerk mit dem ISO-Image von Windows Server 2022 eingebunden.

```
Add-VMDvdDrive -VMName ws2022 -Path D:\ISOs\Ubuntu2004.ISO
```

Die neu erstelle VM lässt sich jetzt mit dem cmdlet **Start-VM ws2022** starten.
Ich fand es jedoch sinnvoller die VM über die GUI des Hyper-V Managers zu starten. Hier hat man am Anfang des Bootvorgangs nur wenige Sekunden Zeit, um das Booten über das eingebundene ISO-Image zu bestätigen.

![vm-boot.png]({{site.baseurl}}/images/vm-boot.png)

Anschließend startet der Installationsprozess für Windows Server 2022.

![ws2022-setup.png]({{site.baseurl}}/images/ws2022-setup.png)

Wenn ihr zu dem Thema Fragen oder Anregungen habt, kontaktiert mich gerne per E-Mail oder über [Mastodon](https://social.tchncs.de/@mialikescoffee).
