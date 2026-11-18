# 🔌 LS 1 – Thema 9: PC-Schnittstellen (Symmetrisch vs. Asymmetrisch inkl. Impulsdiagramm, RS232, SATA, M.2, NVMe)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] **Symmetrische (differenzielle) vs. Asymmetrische (massebezogene) Signalübertragung** (RJ45, USB vs. RS-232) **inklusive Impulsdiagramm!**
- [ ] Definition, Abkürzungen, Funktion & Einsatzgebiete von: **RJ45, RS-232, USB, 3,5mm-Klinke, IDE/PATA, SATA, M.2, NVMe**
- [ ] **Pinbelegung** einer Schnittstelle (RS-232 DE-9)

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Asymmetrische vs. Symmetrische (Differenzielle) Signalübertragung

Warum können moderne Schnittstellen wie **USB, RJ45-Ethernet, SATA und PCIe** gigabitweise Daten fehlerfrei übertragen, während alte Schnittstellen wie **RS-232** extrem langsam und störanfällig bei langen Kabeln sind?

| Kriterium | Asymmetrische Übertragung (*Single-Ended*, z. B. RS-232, Klinke, VGA) | Symmetrische / Differenzielle Übertragung (z. B. USB, RJ45 Ethernet, SATA, PCIe) |
| :--- | :--- | :--- |
| **Leitungsaufbau** | **1 Signalleitung + 1 gemeinsame Masseleitung (`GND = 0 V`)**. | **2 verdrillte Signalleitungen pro Signal (`Signal +` und `Signal -`)**, die exakt **entgegengesetzte (gespiegelte)** Spannungen führen! |
| **Wie misst der Empfänger?** | Misst den absoluten Spannungsunterschied zwischen der Signalleitung und der Masse (`GND`). | Bildet die **Differenz zwischen beiden Leitungen**: $\mathbf{U_{\text{Diff}} = U_+ - U_-}$ (unabhängig von der Masse!). |
| **Was passiert bei einem Störimpuls von außen?** | Die Störspannung $+S$ addiert sich direkt auf die Signalleitung $\rightarrow$ **Signal wird verfälscht (Bitfehler!)**. | Da beide Adern eng miteinander verdrillt sind (*Twisted Pair*), wirkt die Störung $+S$ auf **beide Drähte exakt gleich stark**. Bei der Differenzbildung im Empfänger **hebt sich die Störung zu 0 auf**: $(U_+ + S) - (U_- + S) = U_+ - U_-$! |

```text
IMPULSDIAGRAMM: WARUM SYMMETRISCHE (DIFFERENZIELLE) ÜBERTRAGUNG STÖRUNGEN ELIMINIERT!

                Äußerer Störimpuls (+S) trifft auf das Kabel!
                       ⚡
           +-------+   /\   +-------+
Leitung D+ |  +1V  |  /  \  |  +1V  |   (Original-Signal)
  (0V) ----+       +-/----\-+       +---------
                    /      \
  (0V) ----+       +--------+       +---------
Leitung D- |  -1V  |   /\   |  -1V  |   (Gespiegeltes Signal auf dem 2. Draht, wird durch ⚡
           +-------+  /  \  +-------+    in dieselbe Richtung nach oben gezogen!)

=================================================================================
EMPFÄNGER BERECHNET DIE DIFFERENZ:  U_Diff = (Leitung D+) - (Leitung D-)
=================================================================================

           +------------------------+
Ergebnis   |  +2V   (Störung ⚡ hat |   --> Rechnung: (+1V + S) - (-1V + S) = +2V !
U_Diff     |         sich komplett  |       Das empfangene Signal ist zu 100 % störungsfrei!
  (0V) ----+         weggekürzt!)   +---------
```

---

### 2. Pinbelegung von RS-232 (9-poliger Sub-D-Stecker DE-9)

Wird heute noch für die **Erstkonfiguration von Cisco-Switches/Routern (Konsolenkabel)**, Industrieanlagen und Kassen genutzt:

```text
   Pin 1   Pin 2   Pin 3   Pin 4   Pin 5
  \-------------------------------------/
   \  (1)     (2)     (3)     (4)     (5)  /
    \     (6)     (7)     (8)     (9)     /
     \-----------------------------------/
         Pin 6   Pin 7   Pin 8   Pin 9
```
* **Pin 2 (`RxD` – *Receive Data*):** Empfangsleitung (Daten kommen rein)
* **Pin 3 (`TxD` – *Transmit Data*):** Sendeleitung (Daten gehen raus)
* **Pin 5 (`GND` – *Signal Ground*):** Gemeinsame Signalmasse (Bezugspotenzial $0\text{ V}$)
* *(Steuerleitungen für Flusskontrolle: Pin 1 `DCD`, Pin 4 `DTR`, Pin 6 `DSR`, Pin 7 `RTS` Request to Send, Pin 8 `CTS` Clear to Send, Pin 9 `RI` Ring Indicator)*

---

### 3. Überblick über alle geforderten PC-Schnittstellen

| Schnittstelle | Ausgeschriebene Bedeutung | Funktion & Typische Geräte |
| :--- | :--- | :--- |
| **RJ45 (8P8C)** | *Registered Jack 45 (8 Positions, 8 Contacts)* | 8-poliger Netzwerkstecker für Twisted-Pair-Kupferkabel (Ethernet LAN, bis $10\text{ Gbit/s}$, unterstützt PoE). |
| **3,5mm-Klinke** | *TRS / TRRS (Tip, Ring, Sleeve)* | Analoge asymmetrische Audioschnittstelle für Kopfhörer, Lautsprecher und Mikrofone (3-polig TRS = Stereo; 4-polig TRRS = Stereo + Mikrofon für Headsets). |
| **IDE / PATA** | *Integrated Drive Electronics / Parallel ATA* | Veraltete parallele Schnittstelle (40/80-poliges Flachbandkabel, max. $133\text{ MB/s}$) für alte Festplatten und DVD-Laufwerke. |
| **SATA** | *Serial Advanced Technology Attachment* | Serielle Punkt-zu-Punkt-Schnittstelle (7-poliges Datenkabel, SATA III max. $6\text{ Gbit/s} \approx \mathbf{600\text{ MB/s}}$) für 2,5"-SSDs, 3,5"-HDDs und optische Laufwerke. |
| **M.2** | *Next Generation Form Factor (NGFF)* | **Mechanischer Steckplatz-Formfaktor** direkt auf dem Mainboard für kompakte Steckkarten (SSDs in Länge 2280 = $22 \times 80\text{ mm}$, WLAN-Module). Kann intern über SATA oder PCIe angebunden sein! |
| **NVMe** | *Non-Volatile Memory Express* | **Hochgeschwindigkeits-Zugriffsprotokoll** für Flash-Speicher (SSDs) direkt über den **PCIe-Bus** (löst das alte, für langsame Magnetfestplatten entwickelte AHCI-Protokoll ab; schafft bis zu 64.000 parallele Befehlswarteschlangen!). |
