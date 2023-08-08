---
published: true
description: Reguläre Ausdrücke (RegEx) sind mächtige Mustererkennungswerkzeuge in Bash. Entdecke, wie sie in if-Abfragen genutzt werden, um Muster abzugleichen und Probleme zu lösen, wie z.B. Validierung von Benutzereingaben und Extraktion von Textelementen.
tags:
  - linux
  - opensource
  - macos
title: RegEx in if-Anweisungen - Effektive Mustererkennung in Bash
---

Reguläre Ausdrücke oft als RegEx abgekürzt (Regular Expressions) sind speizelle Zeichenfolgen zur Mustererkennung. In Kombination mit Bash bilden sie ein äußerst leistungsfähiges Werkzeug. Das Wissen darüber ist besonders für Administratoren, die in Linux oder anderen unixoiden Systemen mit Bash arbeiten, äußerst nützlich, um Probleme zu lösen.

![Bash Logo]({{site.baseurl}}/images/bash_logo.png)

Besonders in if-Abfragen erweisen sich reguläre Ausdrücke als äußerst hilfreich. Sie können beispielsweise zur Validierung von Benutzereingaben oder zur Extraktion und anschließenden Bearbeitung bestimmter Elemente aus einem Text genutzt werden.

## Syntax

Um RegEx in einer if-Abfrage zu verwenden, muss man den Operator '=~' verwenden. 
Damit lässt sich ein String mit einem regulären Ausdruck abgleichen.

```bash
if [[ "$input" =~ regex ]]; then
  # Anweisung, wenn $input dem regulären Ausdruck entspricht.
else
  # Anweisung, wenn $input nicht dem regulären Ausdruck entspricht.
fi
```
Diese If-Anweisung prüft den Inhalt der Variable '$input' gegen ein hier nicht näher definierten regulären Ausdruck. 

Zur Veranschaulichung habe ich einige konkrete Anwendungsbeispiele:

## Prüfen, ob $input einer Zahl entspricht

Im folgenden Beispiel wird in der if-Anweisung geprüft, ob der Inhalt von '$input' einer ganzen Zahl entspricht.

```bash
if [[ "$input" =~ ^[0-9]+$ ]]; then
  # Anweisung, wenn $input eine ganze Zahl ist.
else
  # Anweisung, wenn $input keine ganze Zahl ist.
fi
```

## Prüfen, ob $input aus Buchstaben besteht

Mit diesem regulären Ausdrück kann geprüft werden, ob der Inhalt von '$input' ausschließlich aus Klein- und Großbuchstaben besteht.

```bash
if [[ "$input" =~ ^[A-Za-z]+$ ]]; then
  # Anweisung, wenn $input aus Klein- und Großbuchstaben besteht.
else
  # Anweisung, wenn $input nicht aus Klein- und Großbuchstaben besteht.
fi
```

Die beiden Beispiele sind vergleichsweise einfache Anwendungen von regulären Ausdrücken.
Diese können jedoch rasch komplexer werden. Für die Erstellung eigener regulärer Ausdrücke finde ich die Webseite [regex101.com](https://regex101.com/) äußerst hilfreich. Neben einer umfassenden Dokumentation bietet sie auch die Möglichkeit, eigene reguläre Ausdrücke mit eigenen Eingaben zu testen und zu überprüfen.