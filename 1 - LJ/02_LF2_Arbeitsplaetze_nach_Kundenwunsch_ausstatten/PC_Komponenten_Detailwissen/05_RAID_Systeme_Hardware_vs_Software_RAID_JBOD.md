# 🗄️ LS 1 – Thema 5: RAID-Systeme (Prinzip, Levels, Hardware-RAID vs. Software-RAID & JBOD)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten  
> **AP1-Statistik:** RAID & JBOD brachten in den bisherigen AP1-Prüfungen **17 Punkte**!

---

## ✅ Lehrinhalte nach LS 1 & AP1
- [ ] Grundprinzip von RAID (*Redundant Array of Independent Disks*) & Abgrenzung zum Backup
- [ ] **RAID-Level (RAID 0, 1, 5, 6, 10)** sowie **JBOD** & **Hot-Spare** (Kapazität, Mindestanzahl, Ausfallsicherheit)
- [ ] Technischer Vergleich: **Hardware-RAID vs. Software-RAID vs. Host-/Fake-RAID**

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Die RAID-Level & JBOD im Direktvergleich
*(mit $n$ = Anzahl gleich großer Festplatten, $K$ = Kapazität einer einzelnen Festplatte)*

| Level | Verfahren | Min. Platten ($n$) | Nettokapazität | Darf ausfallen | Lese- / Schreibtempo | Besonderheit |
| :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| **JBOD** | *Just a Bunch of Disks* (Aneinanderhängen / Spanning) | $1\text{–}2$ | $\sum K_i$ ($100\,\%$) | **0 Platten** | Normal (wie 1 Einzelplatte) | Einziges Verfahren, das **unterschiedlich große Platten ohne Kapazitätsverlust** kombiniert; keine Redundanz, kein Striping-Tempovorteil. |
| **RAID 0** | **Striping** (Blockweise Aufteilung ohne Parität) | $2$ | $n \cdot K$ ($100\,\%$) | **0 Platten** (1 Defekt = Totalverlust!) | **Sehr hoch / Sehr hoch** | Kein echtes „Redundant“ Array! Nur für temporäre Scratch-/Cache-Daten. |
| **RAID 1** | **Mirroring** (Vollspiegelung) | $2$ | $1 \cdot K$ (bzw. $\frac{n}{2} \cdot K = 50\,\%$) | **1 Platte** (bzw. $n-1$) | Hoch / Normal | Einfachste Redundanz für 2-Bay-NAS und Betriebssystem-Laufwerke. |
| **RAID 5** | **Striping mit einfach verteilter Parität (XOR)** | $3$ | $(n - 1) \cdot K$ | **1 beliebige Platte** | Sehr hoch / Mittel (wegen Paritätsberechnung) | Beliebtester Standard für Dateiserver; bei Ausfall einer Platte wird aus den übrigen per **XOR** rekonstruiert. |
| **RAID 6** | **Striping mit doppelt verteilter Parität** | $4$ | $(n - 2) \cdot K$ | **2 beliebige Platten gleichzeitig** | Sehr hoch / Etwas langsamer | Hohe Sicherheit bei großen Festplatten ($\ge 8\text{ TB}$), da ein Rebuild viele Stunden dauert. |
| **RAID 10** *(RAID 1+0)* | Erst spiegeln (**RAID 1**), darüber stripen (**RAID 0**) | $4$ *(gerade)* | $\frac{n}{2} \cdot K$ ($50\,\%$) | **Mind. 1 Platte** (bis zu $\frac{n}{2}$, solange kein Spiegelpaar komplett stirbt) | **Sehr hoch / Sehr hoch** | Keine aufwendige Paritätsberechnung nötig $\rightarrow$ extrem schneller Rebuild, ideal für **Datenbankserver**! |

> **Wichtig:** Wenn $n$ **unterschiedlich große** Festplatten zu einem RAID (0, 1, 5, 6, 10) zusammengeschlossen werden, bestimmt immer die **KLEINSTE Festplatte ($K_{\min}$)** die nutzbare Größe aller anderen Platten im Verbund!

---

### 2. Hardware-RAID vs. Software-RAID (Kernfokus in BfK-I!)

| Kriterium | Hardware-RAID (Dedizierter Controller) | Software-RAID (Im Betriebssystem) |
| :--- | :--- | :--- |
| **Wer berechnet Striping & Parität?** | Eigener **RAID-Prozessor (ROC)** auf einer separaten **PCIe-Steckkarte** mit eigenem **DRAM-Cache** (z. B. 2–8 GB). | Die **Haupt-CPU des Servers/PCs** über das Betriebssystem (z. B. Windows Speicherplätze, Linux `mdadm` / ZFS). |
| **Belastung von CPU & RAM des Servers** | **0 % Belastung** (komplett ausgelagert auf die Controller-Karte). | **Vorhanden** (insb. bei schreibintensivem RAID 5/6 muss die Haupt-CPU ständig XOR-Paritäten berechnen). |
| **Schutz des Schreib-Caches bei Stromausfall** | Besitzt oft eine **BBU (*Battery Backup Unit*)** oder ein **Flash-Backup-Modul (FBWC)** direkt auf der Karte: Bei Stromausfall bleiben Daten im Controller-Cache tagelang erhalten! | Kein eigener Batterieschutz auf dem RAM $\rightarrow$ ohne externe **USV** droht bei Stromausfall Datenkorruption (*Write Hole*). |
| **Verhalten bei Ausfall des Controllers / Mainboards** | **Großer Nachteil:** Stirbt die RAID-Controller-Karte selbst, kann man die Platten **nur mit einem baugleichen/kompatiblen Controller desselben Herstellers** wieder auslesen! | **Großer Vorteil:** Hardwareunabhängig! Man kann die Platten in jeden beliebigen anderen PC stecken und das RAID mit Linux/Windows sofort wieder einbinden. |
| **Anschaffungskosten** | **Hoch** ($300\text{ € bis }>1.000\text{ €}$ allein für die Controller-Karte). | **Kostenlos** (im Betriebssystem enthalten). |

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Ein Kunde baut 2 Festplatten à 4 TB und 2 Festplatten à 8 TB in seinen Server ein und konfiguriert daraus ein einziges RAID 5 über alle 4 Platten. Wie groß ist die nutzbare Nettokapazität, warum geht dabei Speicherplatz verloren und wie könnte man die 4 Platten sinnvoller konfigurieren?“*
   * **Lösung:** Im gemeinsamen RAID 5 richtet sich jede Platte nach der **kleinsten Platte ($K_{\min} = 4\text{ TB}$)**. Damit beträgt die Nettokapazität nur $(4 - 1) \times 4\text{ TB} = \mathbf{12\text{ TB}}$ (von den insgesamt $24\text{ TB}$ Bruttokapazität liegen auf den beiden $8\text{-TB}$-Platten jeweils $4\text{ TB}$ ungenutzt brach!). Sinnvoller wäre es, **zwei getrennte RAID-1-Verbünde** anzulegen: Ein RAID 1 aus den beiden $4\text{-TB}$-Platten ($= 4\text{ TB}$ netto) und ein zweites RAID 1 aus den beiden $8\text{-TB}$-Platten ($= 8\text{ TB}$ netto) – das ergibt ebenfalls **$12\text{ TB}$ Nettokapazität**, bietet aber eine viel schnellere Wiederherstellung ohne Paritätsberechnung und verträgt den gleichzeitigen Ausfall je einer 4-TB- und einer 8-TB-Platte!
2. **Frage 2:** *„Wie funktioniert die Paritätsberechnung bei RAID 5 technisch auf Bit-Ebene (Stichwort: XOR)?“*
   * **Lösung:** RAID 5 nutzt die logische **Exklusiv-Oder-Verknüpfung (`XOR` / $\oplus$)**. `XOR` liefert immer `1`, wenn die beiden Eingangsbits unterschiedlich sind, und `0`, wenn sie gleich sind. Steht auf Platte 1 das Bit `1`, auf Platte 2 das Bit `0` und auf Platte 3 das Bit `1`, berechnet sich das Paritätsbit auf Platte 4 als $1 \oplus 0 \oplus 1 = \mathbf{0}$ (die Summe der Einsen ist immer gerade). Fällt nun z. B. Platte 2 aus (`?`), rechnet der Controller einfach die übrigen Platten inkl. Parität per XOR zusammen: $1 \oplus 1 \oplus 0 = \mathbf{0}$ und kennt sofort den fehlenden Wert von Platte 2!
