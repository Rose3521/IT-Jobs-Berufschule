# 💽 LS 1 – Thema 10: Festplatten (HDD vs. SSD, NAND-Flash SLC–QLC, Schreibzyklen, TRIM & ECC)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] Technischer Vergleich & Aufbau: **HDD vs. SSD** (inkl. SATA vs. M.2 NVMe PCIe)
- [ ] **NAND-Flash-Zelltypen (SLC, MLC, TLC, QLC):** Bits pro Zelle, Spannungsniveaus & begrenzte Schreibzyklen
- [ ] SSD-Technologien: **Wear Leveling, TRIM, Garbage Collection, Over-Provisioning**
- [ ] **Fehlererkennung und -korrektur:** ECC (Error Correcting Code) & S.M.A.R.T.

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Technischer Vergleich: HDD vs. SSD

| Merkmal | HDD (*Hard Disk Drive*) | SSD (*Solid State Drive*) |
| :--- | :--- | :--- |
| **Speicherprinzip** | **Magnetisch-mechanisch:** Rotierende Magnetscheiben (*Platter*, $5.400/7.200\text{ U/min}$) + schwebender Schreib-/Lesekopf am Aktuatorarm. | **Elektronisch (Halbleiter):** Nicht-flüchtige **NAND-Flash-Speicherzellen** (*Floating-Gate*- / *3D-Charge-Trap*-Transistoren) + Controller-Chip. |
| **Zugriffszeit (Latenz)** | **Langsam ($\approx 8\text{–}12\text{ ms}$):** Kopf muss mechanisch zur Spur fahren (*Seek Time*) und auf den Sektor warten (*Rotational Latency*). | **Extrem schnell ($\approx 0{,}05\text{ ms}$):** Rein elektronische Adressierung ohne jede Mechanik ($\rightarrow$ über 100-mal höhere **IOPS**!). |
| **Datenrate (Sequenziell)** | ca. **$150\text{–}250\text{ MB/s}$** (SATA) | SATA SSD: ca. **$550\text{ MB/s}$**<br>M.2 PCIe 4.0 NVMe: bis **$7.500\text{ MB/s}$** (PCIe 5.0 bis $14.500\text{ MB/s}$) |
| **Mechanische Robustheit** | **Empfindlich gegen Stöße** im Betrieb (Gefahr eines **Headcrashs** = Lesekopf schlägt auf die Magnetscheibe auf!). | **Absolut stoßfest**, völlig geräuschlos ($0\text{ dB}$), geringerer Stromverbrauch. |
| **Verschleiß durch Schreiben** | Nahezu unbegrenzt wiederbeschreibbar (verschleißt eher am Spindelmotor/Lager). | **Begrenzte Schreib-/Löschzyklen (*P/E-Cycles*, angegeben als TBW = *Terabytes Written*)** pro Flash-Zelle! |

---

### 2. NAND-Flash-Technologien: SLC vs. MLC vs. TLC vs. QLC

Eine Flash-Zelle speichert Informationen als elektrische Ladung (Elektronen) in einer isolierten Kammer. Je mehr Bits man in **eine einzige Zelle** packt, desto mehr feine **Spannungsstufen ($2^n$)** muss der Controller unterscheiden können:

| Zelltyp | Bits / Zelle | Spannungsstufen ($2^n$) | Schreibtempo | Lebensdauer (Schreibzyklen pro Zelle) | Preis & Einsatz |
| :--- | :---: | :---: | :--- | :---: | :--- |
| **SLC** (*Single-Level*) | **1 Bit** | $2^1 = \mathbf{2}$ | **Extrem hoch** | **ca. 50.000 – 100.000 Zyklen** | Sehr teuer; Industrie & als schneller **SLC-Puffercache** auf TLC/QLC-SSDs |
| **MLC** (*Multi-Level*) | **2 Bit** | $2^2 = \mathbf{4}$ | Hoch | **ca. 3.000 – 10.000 Zyklen** | Teuer; Enterprise-Server |
| **TLC** (*Triple-Level*) | **3 Bit** | $2^3 = \mathbf{8}$ | Gut | **ca. 1.000 – 3.000 Zyklen** | **Standard** für gute PC-/Notebook-SSDs |
| **QLC** (*Quad-Level*) | **4 Bit** | $2^4 = \mathbf{16}$ | **Langsam** (sobald der SLC-Cache voll ist!) | **nur ca. 300 – 1.000 Zyklen** | Günstige Großraum-SSDs (Archiv/Leselaufwerk) |

---

### 3. Warum kann eine SSD nicht einfach überschreiben? (Pages, Blocks, TRIM & Wear Leveling)
* **Das physikalische Grundproblem von NAND-Flash:**
  * Lesen und Schreiben erfolgt in kleinen **Seiten (*Pages*, z. B. $4\text{–}16\text{ KB}$)**.
  * Eine bereits beschriebene Page kann aber **NICHT direkt überschrieben** werden – sie muss vorher gelöscht werden!
  * **Das Löschen funktioniert jedoch nur in ganzen Blöcken (*Blocks* = z. B. 256 Pages = $4\text{ MB}$ auf einmal)!**
* **Die 4 Controller-Technologien zur Lösung und Lebensdauerverlängerung:**
  1. **TRIM-Befehl:** Löscht der Nutzer eine Datei in Windows, sendet das Betriebssystem sofort den **TRIM-Befehl** an die SSD: *„Diese Pages sind ab sofort ungültig und können beim Aufräumen gelöscht werden.“*
  2. **Garbage Collection (Hintergrund-Müllabfuhr):** Kopiert in Ruhephasen die noch gültigen Pages eines teilweise gefüllten Blocks in einen neuen Block um und löscht den alten Block komplett frei, damit beim nächsten Speichern sofort leere Pages bereitstehen.
  3. **Wear Leveling (Verschleißausgleich):** Der SSD-Controller verteilt Schreibzugriffe gleichmäßig über alle physischen Speicherblöcke, damit nicht einzelne Zellen vorzeitig ihre maximale Zykluszahl erreichen.
  4. **Fehlererkennung & -korrektur (ECC & S.M.A.R.T.):**
     * **ECC (*Error Correcting Code*, z. B. LDPC):** Da die Isolierschicht mit den Jahren altert und Elektronen entweichen können, speichert der Controller zu jedem Datenblock Prüfbits mit, um gekippte Bits beim Lesen automatisch im Flug zu korrigieren. Defekte Blöcke werden gesperrt und durch unsichtbare Reserveblöcke (**Over-Provisioning**, ca. $7\text{–}10\,\%$ der SSD) ersetzt.
     * **S.M.A.R.T. (*Self-Monitoring, Analysis and Reporting Technology*):** Überwacht Temperatur, Betriebsstunden, verbrauchte Reserveblöcke und verbleibende Lebensdauer (TBW) und warnt vor einem drohenden Ausfall.

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Warum bricht bei einer günstigen 2-TB-QLC-SSD beim Kopieren einer 200 GB großen Videodatei nach den ersten 30 GB die Schreibgeschwindigkeit plötzlich von 5.000 MB/s auf magere 120 MB/s (langsamer als eine HDD!) ein?“*
   * **Lösung:** Weil das Unterscheiden von 16 feinen Spannungszuständen bei QLC extrem langsam ist, betreibt der SSD-Controller einen Teil der Zellen im Alltag als schnellen **dynamischen SLC-Cache (1 Bit pro Zelle)**. Solange die Datei in diesen z. B. 30 GB großen SLC-Cache passt, schreibt die SSD mit $5.000\text{ MB/s}$. Ist der SLC-Cache jedoch vollgeschrieben, muss der Controller gleichzeitig den Cache leeren und direkt im langsamen **4-Bit-QLC-Modus** schreiben, wodurch die Datenrate dramatisch einbricht!
2. **Frage 2:** *„Warum sollte man eine SSD niemals bis auf das letzte Megabyte (99–100 %) vollpacken?“*
   * **Lösung:** Wenn kaum noch freie Blöcke vorhanden sind, hat der Controller keinen Spielraum mehr für **Wear Leveling**, **SLC-Caching** und die **Garbage Collection** (er muss für jede kleine Änderung ständig fast volle Blöcke hin- und herkopieren $\rightarrow$ hohe *Write Amplification*). Dadurch wird die SSD extrem langsam und die wenigen verbleibenden freien Blöcke verschleißen im Zeitraffer!
