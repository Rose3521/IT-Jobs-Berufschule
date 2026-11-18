# 🗄️ LF 5 – Teil 4: Datenbankmodellierung (ERM), Relationenmodell & Normalisierung (1NF–3NF)

> **Lernfeld 5:** Software zur Verwaltung von Daten anpassen (Datenbanken)  
> **AP1-Statistik:** *ERM (Entity-Relationship-Modell)* steht mit **42 Gesamtpunkten (55 % Häufigkeit) auf Platz 2 aller AP1-Einzelthemen**! In der AP2 FIAE brachten *ERM, Relationales Datenmodell & Anomalien/Normalisierung* weitere **247 Punkte**!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Anomalien** (Einfüge-, Änderungs-, Löschanomalie) in unnormalisierten Tabellen erklären
- [ ] **ERM (Entity-Relationship-Modell):** Entitätstypen, Attribute, Primärschlüssel und Beziehungen zeichnen
- [ ] **Kardinalitäten (`1:1`, `1:n`, `n:m`)** in Chen- und Krähenfuß-Notation bestimmen
- [ ] Überführung eines ERM in das **Relationenmodell (Tabellenschema mit PK & FK, inkl. `n:m`-Zwischentabelle)** durchführen
- [ ] Eine Tabelle Schritt für Schritt in die **1., 2. und 3. Normalform (1NF, 2NF, 3NF)** überführen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Warum Datenbanken normalisieren? Redundanzen & Die 3 Anomalien
Wenn Daten mehrfach doppelt (**redundant**) in einer einzigen großen Tabelle gespeichert werden, treten **3 Datenbank-Anomalien** auf:
1. **Änderungsanomalie (*Update Anomaly*):** Ändert sich z. B. die Telefonnummer eines Lieferanten, der in 50 Zeilen steht, und man vergisst eine Zeile, entstehen widersprüchliche (**inkonsistente**) Daten!
2. **Einfügeanomalie (*Insertion Anomaly*):** Ein neuer Lieferant kann noch gar nicht in die Tabelle eingetragen werden, solange man noch keinen Artikel bei ihm bestellt hat (weil der Primärschlüssel `Bestell_Nr` sonst leer / `NULL` wäre!).
3. **Löschanomalie (*Deletion Anomaly*):** Löscht man die letzte Bestellung eines Kunden aus der Tabelle, gehen damit ungewollt auch alle Stammdaten (Name, Adresse) dieses Kunden komplett verloren!

---

### 2. Vom ER-Modell (ERM) zum Relationenmodell (PK & FK Regeln!)

* **Symbole im klassischen Chen-ERM:**
  * **Rechteck:** **Entitätstyp** (z. B. `Kunde`, `Auftrag`, `Artikel` – immer im Singular!).
  * **Ellipse (Oval):** **Attribut** (Eigenschaft, z. B. `Kunden_ID`, `Nachname`; der **Primärschlüssel wird unterstrichen**: <u>`Kunden_ID`</u>).
  * **Raute (Diamant):** **Beziehung (*Relationship*)** zwischen zwei Entitätstypen (Verb, z. B. `erteilt`, `enthält`).

#### Die 3 goldenen Überführungsregeln ins Tabellenmodell (Relationenmodell):

| Kardinalität | Beispiel | Regel für Primärschlüssel (PK) & Fremdschlüssel (FK) |
| :--- | :--- | :--- |
| **`1 : 1`** | `1 Mitarbeiter` hat `1 Dienstwagen` | Der PK der einen Tabelle wird als FK in die andere Tabelle aufgenommen (oder beide werden in einer Tabelle zusammengefasst). |
| **`1 : n`** | `1 Kunde` (`1`) erteilt `mehrere Aufträge` (`n`) | **Merksatz:** **Der Primärschlüssel der `1`-Seite wandert IMMER als Fremdschlüssel (FK) in die Tabelle der `n`-Seite!**<br>*Schema:*<br>`Kunde` (<u>`Kunden_ID`</u>, `Nachname`, `Vorname`)<br>`Auftrag` (<u>`Auftrags_ID`</u>, `Datum`, *`FK_Kunden_ID`*) |
| **`n : m`** | `1 Auftrag` (`n`) enthält `mehrere Artikel` (`m`), und `1 Artikel` kommt in `mehreren Aufträgen` vor | **Merksatz:** **Eine `n:m`-Beziehung MUSS zwingend durch eine DRITTE ZWISCHENTABELLE (Verknüpfungstabelle) in zwei `1:n`-Beziehungen aufgelöst werden!**<br>Die Zwischentabelle erhält die Primärschlüssel beider Tabellen als Fremdschlüssel (die zusammen den zusammengesetzten PK bilden) plus eventuelle Beziehungsattribute (wie `Bestellmenge`):<br>*Schema:*<br>`Auftrag` (<u>`Auftrags_ID`</u>, `Datum`)<br>`Artikel` (<u>`Artikel_ID`</u>, `Bezeichnung`, `Listenpreis`)<br>**`Auftragsposition`** (<u>*`FK_Auftrags_ID`*</u>, <u>*`FK_Artikel_ID`*</u>, `Menge`) |

---

### 3. Die 3 Normalformen (1NF, 2NF, 3NF) – Das Kochrezept für jede Prüfung!

```text
UNNORMALISIERTE AUSGANGSTABELLE (0NF):
[ Bestell_ID | Datum | Kunde_Name | PLZ_Ort        | Artikel_Liste (Wiederholgruppe!)          ]
[ 1001       | 12.05.| Max Müller | 70173 Stuttgart| 2x Maus (15€), 1x Tastatur (40€)          ]
```

#### Schritt 1: Erste Normalform (1NF) – „Atomare Werte & keine Wiederholgruppen“
* **Regel:** Jedes Attribut darf nur **atomare (unteilbare) Einzelwerte** enthalten, und es darf **keine Aufzählungen oder Wiederholgruppen** (`Artikel1, Artikel2`) in einer Zeile geben.
* **Was du tun musst:**
  1. Zusammengesetzte Spalten aufspalten: `Kunde_Name` $\rightarrow$ `Vorname`, `Nachname`; `PLZ_Ort` $\rightarrow$ `PLZ`, `Ort`.
  2. Wiederholgruppen in **einzelne Zeilen** untereinander schreiben und den **zusammengesetzten Primärschlüssel** markieren:
     * `Bestellung_1NF` (<u>`Bestell_ID`</u>, <u>`Artikel_ID`</u>, `Datum`, `Kunden_ID`, `Vorname`, `Nachname`, `PLZ`, `Ort`, `Artikelname`, `Einzelpreis`, `Menge`)

#### Schritt 2: Zweite Normalform (2NF) – „Volle funktionale Abhängigkeit vom GESAMTEN Primärschlüssel“
* **Regel:** Tabelle ist in **1NF** **UND** jedes Nicht-Schlüsselattribut hängt vom **gesamten** zusammengesetzten Primärschlüssel `(Bestell_ID, Artikel_ID)` ab – nicht nur von einem Teil davon! *(Hinweis: Hat eine Tabelle in 1NF bereits einen einfachen Primärschlüssel aus nur 1 Spalte, ist sie automatisch schon in 2NF!).*
* **Was du tun musst:**
  * Alles, was **nur von `Bestell_ID`** abhängt (`Datum`, `Kunde`), kommt in die Tabelle `Bestellung`.
  * Alles, was **nur von `Artikel_ID`** abhängt (`Artikelname`, `Einzelpreis`), kommt in die Tabelle `Artikel`.
  * In der Zwischentabelle `Bestellposition` bleibt nur das übrig, was **von beiden zusammen** abhängt (`Menge`!):
    * `Bestellung` (<u>`Bestell_ID`</u>, `Datum`, `Kunden_ID`, `Vorname`, `Nachname`, `PLZ`, `Ort`)
    * `Artikel` (<u>`Artikel_ID`</u>, `Artikelname`, `Einzelpreis`)
    * `Bestellposition` (<u>*`FK_Bestell_ID`*</u>, <u>*`FK_Artikel_ID`*</u>, `Menge`)

#### Schritt 3: Dritte Normalform (3NF) – „Keine transitiven Abhängigkeiten zwischen Nicht-Schlüsselattributen“
* **Regel:** Tabelle ist in **2NF** **UND** kein Nicht-Schlüsselattribut hängt von einem anderen Nicht-Schlüsselattribut ab (*indirekte / transitive Abhängigkeit: $\text{PK} \rightarrow \text{Nicht-Schlüssel A} \rightarrow \text{Nicht-Schlüssel B}$*).
* **Was du tun musst:**
  * In `Bestellung` hängen `Vorname`, `Nachname`, `PLZ` von `Kunden_ID` ab (und nicht von `Bestell_ID`!) $\rightarrow$ eigene Tabelle **`Kunde`**!
  * In `Kunde` hängt der `Ort` von der `PLZ` ab (`Kunden_ID` $\rightarrow$ `PLZ` $\rightarrow$ `Ort`) $\rightarrow$ eigene Tabelle **`Orte`**!
  * **Fertiges Schema in 3. Normalform (3NF):**
    1. **`Orte`** (<u>`PLZ`</u>, `Ort`)
    2. **`Kunde`** (<u>`Kunden_ID`</u>, `Vorname`, `Nachname`, `Strasse`, *`FK_PLZ`*)
    3. **`Bestellung`** (<u>`Bestell_ID`</u>, `Datum`, *`FK_Kunden_ID`*)
    4. **`Artikel`** (<u>`Artikel_ID`</u>, `Artikelname`, `Einzelpreis`)
    5. **`Bestellposition`** (<u>*`FK_Bestell_ID`*</u>, <u>*`FK_Artikel_ID`*</u>, `Menge`)
