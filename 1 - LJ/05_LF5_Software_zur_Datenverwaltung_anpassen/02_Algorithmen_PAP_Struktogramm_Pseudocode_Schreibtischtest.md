# 📋 LF 5 – Teil 2: Algorithmen (PAP, Struktogramm, Pseudocode) & Schreibtischtest

> **Lernfeld 5:** Software zur Verwaltung von Daten anpassen (SAE / BFK-S)  
> **AP1-Statistik:** *Schreibtischtest (41 P. – Platz 3 aller Einzelthemen! Kam in den letzten 5 AP1-Prüfungen in Folge dran!) + Struktogramm & Pseudocode (18 P.)* = **59 Punkte** in der AP1 (und **385 Punkte** in der AP2 FIAE)!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Programmablaufplan (PAP nach DIN 66001):** Alle Symbole kennen und zeichnen
- [ ] **Struktogramm (Nassi-Shneiderman nach DIN 66261):** Sequenz, Verzweigung und Schleifen zeichnen
- [ ] **Pseudocode** lesen, verstehen und selbst verfassen
- [ ] **Schreibtischtest (Trace-Tabelle)** fehlerfrei für Schleifen und Arrays durchführen
- [ ] Standard-Algorithmen (**Minimum/Maximum/Durchschnitt in Array**, **Rabattstaffel**, **Bubble-Sort**) beherrschen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Die 3 Darstellungsformen für Algorithmen

#### A. Programmablaufplan (PAP nach DIN 66001)
```text
   ( Start )                   <-- Oval / Abgerundetes Rechteck: Start & Ende
       |
       v
  / Eingabe: n /               <-- Parallelogramm: Ein- oder Ausgabe (Input / Output)
       |
       v
+--------------+
| summe = 0    |               <-- Rechteck: Verarbeitung / Zuweisung / Rechnung
| i = 1        |
+--------------+
       |
       v
     /   \
   / i<=n? \ --- Nein ---> / Ausgabe: summe / ---> ( Ende )
     \   /                 <-- Raute (Diamant): Bedingung / Verzweigung (mit Ja / Nein)
       | Ja
       v
+-------------------+
| summe = summe + i |
| i = i + 1         |
+-------------------+
       |
       +-----> (Pfeil zurück nach oben vor die Raute "i<=n?")
```

#### B. Struktogramm (Nassi-Shneiderman-Diagramm nach DIN 66261)
*(Keine Pfeile! Wird von oben nach unten aus ineinander verschachtelten Rechteck-Blöcken aufgebaut):*

```text
+-------------------------------------------------------------------+
| Eingabe: bestellwert                                              |
+-------------------------------------------------------------------+
|                    bestellwert >= 500 ?                           |
|          Ja                     \              Nein               |
+----------------------------------+--------------------------------+
| rabatt = 0.10                    | rabatt = 0.0                   |
+----------------------------------+--------------------------------+
| endpreis = bestellwert * (1 - rabatt)                             |
+-------------------------------------------------------------------+
| Ausgabe: endpreis                                                 |
+-------------------------------------------------------------------+
```

* **Schleifen im Struktogramm erkennen:**
  * **Kopfgesteuerte Schleife (`Solange / while`):** Der horizontale Balken mit der Bedingung ist **OBEN**, links geht ein senkrechter Randbalken nach unten (ein **L auf dem Kopf** `┏`).
  * **Fußgesteuerte Schleife (`Wiederhole bis / do-while`):** Der horizontale Balken mit der Bedingung steht **UNTEN** unter dem Schleifenrumpf (ein normales **`L`** `┗`).

---

### 2. Der Schreibtischtest (Trace-Tabelle) – Schritt-für-Schritt-Musterbeispiel

> **Prüfungsaufgabe (Typisches AP1-Niveau):**  
> Gegeben ist das Array `preise = [20, 50, 15, 40]` (Länge `n = 4`) und folgender Pseudocode. Führen Sie einen **Schreibtischtest** durch und geben Sie die Konsolenausgabe an!

```text
preise = [20, 50, 15, 40]
summe = 0
max = preise[0]

FÜR i = 0 BIS i < 4 SCHRITTWEITE 1:
    WENN preise[i] > max DANN:
        max = preise[i]
    ENDE WENN
    
    WENN preise[i] >= 20 DANN:
        summe = summe + preise[i]
    ENDE WENN
ENDE FÜR

Ausgabe: "Max:", max, "Summe:", summe
```

#### Ausgefüllte Trace-Tabelle (Schreibtischtest):

| Durchlauf | `i` | Bedingung `i < 4` | `preise[i]` | `preise[i] > max`? | **`max`** (danach) | `preise[i] >= 20`? | **`summe`** (danach) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| *Initialisierung* | – | – | – | – | **`20`** (`preise[0]`) | – | **`0`** |
| **1. Durchlauf** | `0` | `0 < 4` (wahr) | `20` | `20 > 20` (falsch) | `20` | `20 >= 20` (wahr!) | $0 + 20 = \mathbf{20}$ |
| **2. Durchlauf** | `1` | `1 < 4` (wahr) | `50` | `50 > 20` (wahr!) | **`50`** | `50 >= 20` (wahr!) | $20 + 50 = \mathbf{70}$ |
| **3. Durchlauf** | `2` | `2 < 4` (wahr) | `15` | `15 > 50` (falsch) | `50` | `15 >= 20` (falsch) | `70` *(unverändert)* |
| **4. Durchlauf** | `3` | `3 < 4` (wahr) | `40` | `40 > 50` (falsch) | `50` | `40 >= 20` (wahr!) | $70 + 40 = \mathbf{110}$ |
| **Abbruch** | `4` | `4 < 4` (**falsch!**) | *Schleife endet* | – | **`50`** | – | **`110`** |

* **Konsolenausgabe:** `Max: 50 Summe: 110`

---

### 3. Der Klassiker unter den Sortieralgorithmen: Bubble-Sort (mit Dreieckstausch)
Vergleicht in zwei ineinander verschachtelten Schleifen jeweils **zwei direkt benachbarte Elemente (`arr[j]` und `arr[j + 1]`)** und vertauscht sie mithilfe einer Hilfsvariable `temp`, wenn das linke größer als das rechte ist (sodass die größten Zahlen wie Luftblasen nach rechts oben aufsteigen):

```text
FÜR i = 0 BIS n - 2:
    FÜR j = 0 BIS n - 2 - i:
        WENN arr[j] > arr[j + 1] DANN:
            temp = arr[j]          // Dreieckstausch!
            arr[j] = arr[j + 1]
            arr[j + 1] = temp
        ENDE WENN
    ENDE FÜR
ENDE FÜR
```
