# 📐 LF 5 – Teil 3: Objektorientierung (OOP) & UML-Diagramme (Use-Case, Klassen-, Aktivitätsdiagramm)

> **Lernfeld 5:** Software zur Verwaltung von Daten anpassen (SAE / BFK-S)  
> **AP1-Statistik:** *Use-Case-Diagramm (18 P.), Prozessanalyse (16 P.), Klassendiagramm (13 P.), Aktivitätsdiagramm (9 P.)* = **56 Punkte** in der AP1 (und **330 Punkte** in der AP2 FIAE)!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **OOP-Grundbegriffe:** Klasse vs. Objekt (Instanz), Attribute, Methoden, Konstruktor, Sichtbarkeiten (`+`, `-`, `#`), Kapselung, Vererbung, Polymorphie
- [ ] **UML-Use-Case-Diagramm (Anwendungsfalldiagramm):** Akteure, Systemgrenze, `<<include>>` vs. `<<extend>>`
- [ ] **UML-Klassendiagramm:** Notation, Multiplizitäten, **Assoziation, Aggregation ($\diamond$), Komposition ($\blacklozenge$) & Vererbung**
- [ ] **UML-Aktivitätsdiagramm & Prozessanalyse:** Start/Ende, Aktionen, Verzweigungen, **Fork/Join (Parallelisierung)** & Swimlanes

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Objektorientierte Programmierung (OOP) – Grundbegriffe
* **Klasse (*Class*):** Der abstrakte **Bauplan / die Schablone** (z. B. Klasse `Mitarbeiter`), in der festgelegt wird, welche Eigenschaften (**Attribute**) und Fähigkeiten (**Methoden**) Objekte dieses Typs haben.
* **Objekt (*Instanz*):** Ein konkretes, zur Laufzeit im Arbeitsspeicher mit dem Schlüsselwort `new` (über den **Konstruktor**) erzeugtes Exemplar einer Klasse (z. B. `mitarbeiter1 = new Mitarbeiter("Anna Müller", 101)`).
* **Daten-Kapselung (*Encapsulation*) & Sichtbarkeiten in UML:**
  * **`-` (`private`):** Nur innerhalb der eigenen Klasse sichtbar (**Attribute sind fast immer `- private`**!).
  * **`+` (`public`):** Von überall aus sichtbar (**Methoden, Konstruktoren sowie Getter/Setter sind meist `+ public`**!).
  * **`#` (`protected`):** Innerhalb der eigenen Klasse **und in allen erbenden Unterklassen (Kindklassen)** sichtbar!

---

### 2. UML-Anwendungsfalldiagramm (Use-Case-Diagramm – AP1: 18 Punkte!)

Beschreibt **aus Sicht des Anwenders (von außen)**, **WAS** das geplante System leisten soll (ohne technische Details):
* **Elemente:** **Akteur** (Strichmännchen außerhalb des Systems), **Systemgrenze** (großes Rechteck mit Systemnamen oben), **Anwendungsfälle / Use-Cases** (Ovale innerhalb des Rechtecks).
* **Die wichtigste Prüfungsfalle: `<<include>>` vs. `<<extend>>` (Pfeilrichtung beachten!):**

```text
1. <<include>> (ZWINGENDE Einbindung / "Jedes Mal wenn A, dann MUSS auch B"):
   [ Use-Case A (Basis) ] -------- <<include>> --------> [ Use-Case B (Eingebunden) ]
   Beispiel: [ Überweisung ausführen ] ---<<include>>---> [ TAN-Freigabe prüfen ]

2. <<extend>> (OPTIONALE Erweiterung unter einer Bedingung / "Manchmal bei A auch C"):
   [ Use-Case A (Basis) ] <------- <<extend>> ---------- [ Use-Case C (Optional) ]
   Beispiel: [ Warenkorb bezahlen ] <---<<extend>>--- [ Mahngebühr berechnen (falls Verzug) ]
```

---

### 3. UML-Klassendiagramm (AP1: 13 Punkte, AP2: 91 Punkte!)

#### Aufbau eines Klassenkastens (3 Abteile):
```text
+----------------------------------------------------+
|                     Kunde                          |  <-- 1. Klassenname (Substantiv, Einzahl)
+----------------------------------------------------+
| - kundenNr : int                                   |  <-- 2. Attribute: Sichtbarkeit Name : Datentyp
| - name : String                                    |
| - rabattSatz : double = 0.0                        |
+----------------------------------------------------+
| + Kunde(kundenNr : int, name : String)             |  <-- 3. Methoden: Sichtbarkeit Name(Parameter) : Rückgabetyp
| + berechneEndpreis(betrag : double) : double       |
| + getName() : String                               |
+----------------------------------------------------+
```

#### Die 4 Beziehungsarten zwischen Klassen

| Beziehungsart | UML-Symbol | Bedeutung & Lebensdauer-Abhängigkeit | Beispiel |
| :--- | :--- | :--- | :--- |
| **1. Assoziation** | Einfache durchgezogene Linie (`-----`) mit Multiplizitäten (`1`, `0..*`, `1..*`) | *„Kennt / Nutzt“*-Beziehung: Zwei Klassen stehen miteinander in Verbindung. | `Kunde [1] ----- [0..*] Auftrag` |
| **2. Aggregation** *(Schwaches Teil-Ganzes)* | Linie mit **leerer (weißer) Raute $\diamond$** auf der Seite des **Ganzen**! | *„Besteht aus / Hat“*-Beziehung **OHNE Existenzabhängigkeit**: Wird das Ganze gelöscht, **existieren die Einzelteile weiter**! | `Abteilung [0..*] <>----- [1..*] Mitarbeiter` *(Wird die Abteilung aufgelöst, leben die Mitarbeiter weiter).* |
| **3. Komposition** *(Starkes Teil-Ganzes)* | Linie mit **ausgefüllter schwarzer Raute $\blacklozenge$** auf der Seite des **Ganzen**! | *„Besteht zwingend aus“* **MIT strenger Existenzabhängigkeit**: Ein Teil gehört zu maximal **1** Ganzen. **Wird das Ganze gelöscht, sterben alle Einzelteile automatisch mit!** | `Rechnung [1] <*>----- [1..*] Rechnungsposition` *(Ohne Rechnung gibt es keine Rechnungsposition).* |
| **4. Vererbung** *(Generalisierung)* | Durchgezogene Linie mit **leerer geschlossener Dreiecks-Pfeilspitze $\triangleright$** zur **Oberklasse**! | *„Ist ein (is-a)“*-Beziehung: Die Unterklasse erbt alle (`public`/`protected`) Attribute und Methoden der Oberklasse. | `[PKW] ----|> [Fahrzeug]` |

---

### 4. UML-Aktivitätsdiagramm & Prozessanalyse (AP1: 25 Punkte, AP2: 146 Punkte!)

Modelliert **Geschäftsprozesse** und komplexe Abläufe (inkl. **paralleler Schritte** und **Abteilungs-Swimlanes**):
* **Startknoten:** Schwarzer ausgefüllter Punkt ($\bullet$).
* **Aktivitätsende:** Schwarzer Punkt mit Kreis darum ($\odot$ – „Bullseye“).
* **Aktion:** Abgerundetes Rechteck mit Verb im Infinitiv (z. B. *„Warenverfügbarkeit prüfen“*).
* **Verzweigung (*Decision*) & Zusammenführung (*Merge*):** Raute ($\diamond$) mit Bedingungen in eckigen Klammern an den ausgehenden Pfeilen (`[auf Lager]`, `[nicht auf Lager]`). *(Wichtig: An einer Raute geht immer **nur 1** Weg weiter – **ODER**-Verzweigung!).*
* **Aufspaltung (*Fork*) & Synchronisation (*Join*):** Dicker schwarzer Balken ($\blacksquare\blacksquare\blacksquare$)!
  * Nach einem **Fork-Balken** werden alle ausgehenden Pfeile **gleichzeitig (parallel / UND-Verzweigung)** abgearbeitet (z. B. *gleichzeitig „Ware verpacken“ UND „Rechnung per E-Mail senden“*).
  * Vor dem **Join-Balken** wird gewartet, bis **alle** parallelen Pfade abgeschlossen sind!
