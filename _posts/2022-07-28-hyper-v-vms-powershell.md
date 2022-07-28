---
published: false
description: >-
  Mit Powershell lassen sich virtuelle Maschinen für Hyper-V erstellen und
  konfigurieren.
tags:
  - windows
header:
  image: /images/header_switch.jpg
  image_description: Foto eines Switches von pexels.com.
---
## Virtuelle Maschinen in Hyper-V mit Powershell erstellen

Der reguläre Weg Hyper-V VMs ist wohl über die GUI im Hyper-V Manager. In diesem Beitrag zeige ich, wie man virtuelle Maschinen mit Hilfe von PowerShell erstellen kann.

In modernen Windows-Umgebungen lässt Microsoft einem die Wahl Aufgaben entweder über die GUI oder mit Powershell durchzuführen. Ich habe die Erfahrung gemacht, dass Powershell oft die effizientere und bessere Methode ist. Mit Skripts und Templates lassen sich wiederkehrende Aufgaben automatisieren und man kann die dadurch eingesparte Zeit in wichtigere Projekte investieren.

In diesem Beispiel erstelle ich eine VM mit Ubuntu 20.04 LTS erstellen, auf welcher dann zum Beispiel ein Ticketsystem installiert werden kann. Für den Anfang sollten daher erst einmal 2 CPU-Kerne und 4 GB Arbeitsspeicher ausreichend sein. Zusätzlich wird eine virtuelle Festplatte mit einer Kapazität von 100 GB erstellt und der VM zugewiesen.

## Cmdlets und Parameter

* New-VM: Cmdlet zur Erstellung einer neuen VM
* Name: Name der VM
* -MemoryStartUpBytes: Größe des Arbeitsspeichers
* -NewVHDPath: Pfad der virtuellen Festplatte auf dem Hyper-V Host
* -Path: Pfad der VM auf dem Hyper-V Host
* -NewVHDSizeBytes: Größe der VHD
* -Generation: Generation 1 oder 2 der VM (Gen 2 unterstützt UEFI / Secure Boot)

* AddVMDvdDrive: Fügt ein DVD-Laufwerk zu einer VM hinzu
* -VMName: Name der VM, zu welcher das DVD-Laufwerk hinzugefügt werden soll
* -Path: Pfad der ISO-Datei auf dem Host

* Set-VMProcessor: Virtuelle CPUs einer VM konfigurieren
* -Count: Anzahl der CPUs



New-VM -name "ubuntu2004" -MemoryStartupBytes 4GB -NewVHDPath "D:\Hyper-V\Virtual Hard Disks\hostname.vhdx" -Path "D:\Hyper-V\Virtual Machines" -NewVHDSizeBytes 100GB -Generation 2
{: .notice--info}
