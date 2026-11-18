# 📅 BWL & PM – Teil 2: Projektstrukturplan (PSP), Gantt-Diagramm & Netzplantechnik (Kritischer Pfad)

> **AP1-Statistik:** *Netzplan & Kritischer Pfad (37 P. – Platz 4 aller Einzelthemen!) + Gantt-Diagramm (12 P.)* = **49 Punkte** in der AP1!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Projektstrukturplan (PSP)** (funktions-/phasenorientiert vs. objektorientiert) aufbauen und **Arbeitspakete** definieren
- [ ] **Meilensteine** erklären und ein **Gantt-Diagramm (Balkenplan)** zeichnen/auswerten
- [ ] **Netzplan (Vorgangsknotennetzplan nach DIN 69900)** komplett berechnen:
  - [ ] Vorwärtsrechnung (**FAZ, FEZ**)
  - [ ] Rückwärtsrechnung (**SAZ, SEZ**)
  - [ ] **Gesamtpuffer (GP)** & **Freier Puffer (FP)**
  - [ ] **Kritischen Pfad** bestimmen und Auswirkungen von Verzögerungen beurteilen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Projektstrukturplan (PSP) & Balkenplan (Gantt-Diagramm)
* **Projektstrukturplan (PSP / *Work Breakdown Structure*):**
  * Zerlegt das Gesamtprojekt hierarchisch als Baumdiagramm in überschaubare Teilaufgaben und **Arbeitspakete (AP)** (unterste Ebene im PSP, die einer verantwortlichen Person mit Zeit- und Kostenvorgabe zugewiesen wird).
  * **Wichtig:** Ein PSP zeigt nur, **WELCHE** Aufgaben zum Projekt gehören, enthält aber noch **KEINE zeitliche Reihenfolge oder Abhängigkeiten**!
* **Gantt-Diagramm (Balkenplan – AP1 Herbst 2023, 12 Punkte!):**
  * Stellt die Arbeitspakete auf einer **horizontalen Zeitachse (Kalendertage/Wochen)** als Balken dar.
  * **Vorteile:** Man sieht sofort auf einen Blick die **Start- und Endtermine**, die **Dauer** (Länge des Balkens) und welche Vorgänge sich zeitlich **überlappen (parallel laufen)**.
  * **Nachteil gegenüber dem Netzplan:** Bei komplexen Projekten sind die **Pufferzeiten (GP, FP)** und logischen Abhängigkeiten schlechter ablesbar als im Netzplan.
* **Meilenstein:** Ein wichtiges Kontrollereignis im Projekt mit der **Dauer 0** (z. B. *„Pflichtenheft vom Kunden freigegeben“*, *„Server-Hardware geliefert“*), im Gantt-Diagramm meist als **Raute ($\diamond$)** dargestellt.

---

### 2. Netzplantechnik (Vorgangsknotennetzplan) – Komplettanleitung für volle Punktzahl!

#### A. Knoten-Aufbau & Legende
*(Achtung: Prüfe in der IHK-Klausur immer zuerst die abgedruckte Legende des Knotens, da manchmal Dauer und Puffer an leicht anderen Plätzen stehen!)*

```text
+---------------------------------------------------+
|   Vorgangs-Nr. (A, B...)   |      Dauer (D)       |
+---------------------------------------------------+
|             Bezeichnung des Vorgangs              |
+---------------------------------------------------+
|   FAZ (Frühester Anfang)   |  GP  |  FP  |  FEZ   |
|   SAZ (Spätester Anfang)   |      |      |  SEZ   |
+---------------------------------------------------+
```

#### B. Die 6 goldenen Rechenformeln

1. **Vorwärtsrechnung (von links nach rechts $\rightarrow$ bestimmt die minimale Projektdauer):**
   * Beim allerersten Startvorgang gilt: **$\text{FAZ} = 0$**
   * **Frühester Endzeitpunkt:**
     $$\mathbf{\text{FEZ} = \text{FAZ} + \text{Dauer } D}$$
   * **Frühester Anfangszeitpunkt des Nachfolgers:**
     $$\mathbf{\text{FAZ}_{\text{Nachfolger}} = \max(\text{FEZ aller direkten Vorgänger})}$$
     *(Merksatz Vorwärtsrechnung: Laufen mehrere Pfeile in einen Knoten hinein, muss man auf den **langsamsten/spätesten Vorgänger** warten $\rightarrow$ der **GRÖSSTE `FEZ`** gewinnt!)*

2. **Rückwärtsrechnung (von rechts nach links $\leftarrow$ bestimmt die spätesten erlaubten Zeiten):**
   * Beim letzten Endvorgang gilt: **$\text{SEZ} = \text{FEZ}$** (Gesamtprojektdauer)
   * **Spätester Anfangszeitpunkt:**
     $$\mathbf{\text{SAZ} = \text{SEZ} - \text{Dauer } D}$$
   * **Spätester Endzeitpunkt des Vorgängers:**
     $$\mathbf{\text{SEZ}_{\text{Vorgänger}} = \min(\text{SAZ aller direkten Nachfolger})}$$
     *(Merksatz Rückwärtsrechnung: Gehen von einem Knoten mehrere Pfeile nach rechts weg, darf man **keinen einzigen Nachfolger** aufhalten $\rightarrow$ beim Zurückrechnen gewinnt der **KLEINSTE `SAZ`**!)*

3. **Pufferzeiten & Kritischer Pfad:**
   * **Gesamtpuffer ($\text{GP}$):** Zeitspanne, um die sich ein Vorgang maximal verzögern darf, **ohne das Gesamtprojektende zu gefährden**:
     $$\mathbf{\text{GP} = \text{SAZ} - \text{FAZ}} \qquad (\text{Kontrolle: ist immer gleich } \text{SEZ} - \text{FEZ})$$
   * **Freier Puffer ($\text{FP}$):** Zeitspanne, um die sich ein Vorgang verzögern darf, **ohne den frühesten Start ($\text{FAZ}$) irgendeines direkten Nachfolgers zu verzögern**:
     $$\mathbf{\text{FP} = \min(\text{FAZ aller direkten Nachfolger}) - \text{FEZ}_{\text{eigener Knoten}}}$$
   * **Kritischer Pfad:** Die durchgehende Kette aller Vorgänge vom Start bis zum Ende, bei denen **$\text{GP} = 0$ (und $\text{FP} = 0$)** ist! Verzögert sich ein Vorgang auf dem kritischen Pfad auch nur um 1 Tag, verzögert sich sofort das gesamte Projekt um 1 Tag!

---

### 3. Vollständiges Rechenbeispiel eines Netzplans

Gegeben ist folgende Vorgangsliste (Dauer in Arbeitstagen):

| Vorgang | Beschreibung | Dauer ($D$) | Direkte Vorgänger |
| :---: | :--- | :---: | :---: |
| **A** | Bedarfsanalyse durchführen | $3$ | – *(Start)* |
| **B** | Server-Hardware bestellen & liefern | $6$ | A |
| **C** | Netzwerkkabel im Büro verlegen | $2$ | A |
| **D** | Alte Arbeitsplätze abbauen | $1$ | C |
| **E** | Server & Clients installieren | $4$ | B, D |

#### Ausgerechnete Werte für alle Knoten:

| Vorgang | Dauer $D$ | **FAZ** | **FEZ** ($\text{FAZ}+D$) | **SAZ** ($\text{SEZ}-D$) | **SEZ** | **GP** ($\text{SAZ}-\text{FAZ}$) | **FP** ($\text{FAZ}_{\text{Nachf.}}-\text{FEZ}$) | Kritisch? |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **A** | $3$ | **`0`** | **`3`** | **`0`** | **`3`** | **$0$** | $3 - 3 = \mathbf{0}$ | **JA!** |
| **B** | $6$ | **`3`** | **`9`** | **`3`** | **`9`** | **$0$** | $9 - 9 = \mathbf{0}$ | **JA!** |
| **C** | $2$ | `3` | `5` | `6` | `8` | $6 - 3 = \mathbf{3}$ | $5 (\text{FAZ}_D) - 5 = \mathbf{0}$ | Nein |
| **D** | $1$ | `5` | `6` | `8` | `9` | $8 - 5 = \mathbf{3}$ | $9 (\text{FAZ}_E) - 6 = \mathbf{3}$ | Nein |
| **E** | $4$ | **`9`** *($\max(9,6)$)* | **`13`** | **`9`** | **`13`** | **$0$** | **$0$** | **JA!** |

* **Gesamtprojektdauer:** **$13\text{ Arbeitstage}$**
* **Kritischer Pfad:** **`A -> B -> E`**
* **Erklärung des Unterschieds zwischen GP und FP an Vorgang C und D:**
  * Vorgang **C** hat zwar einen **Gesamtpuffer $\text{GP} = 3\text{ Tage}$**, aber einen **Freien Puffer $\text{FP} = 0\text{ Tage}$**, weil direkt danach Vorgang D zum frühesten Zeitpunkt $\text{FAZ}_D = 5$ starten möchte. Erst hinter Vorgang D ($\text{FEZ}_D = 6$) entsteht vor dem Zusammenlaufen mit B ($\text{FAZ}_E = 9$) die echte Wartezeit von **$\text{FP}_D = 9 - 6 = 3\text{ Tagen}$**!
