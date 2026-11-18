# 💾 LS 1 – Thema 3: Speicherbausteine (ROM/EEPROM, SRAM vs. DRAM, SDRAM vs. DDR Impulsdiagramm)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] Übersicht: **Flüchtige vs. nicht-flüchtige Speicher**
- [ ] Nicht-flüchtig: **ROM, PROM, EPROM, EEPROM / Flash**
- [ ] Flüchtig: **SRAM, DRAM, CMOS-RAM** (technischer Aufbau & Merkmale)
- [ ] **SDRAM vs. DDR1/DDR-SDRAM im Vergleich inkl. Impulsdiagramm!**

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Übersicht der Halbleiterspeicher

| Kategorie | Speicherart | Funktionsweise / Löschbarkeit | Typisches Einsatzgebiet |
| :--- | :--- | :--- | :--- |
| **Nicht-flüchtig** *(Daten bleiben ohne Strom erhalten)* | **Masken-ROM** | Werkseitig fest belichtet; Inhalt kann **niemals** verändert werden. | Alte Firmware, Taschenrechner |
| | **PROM** *(veraltet)* | *Programmable ROM*: Einmalig durch gezieltes Durchbrennen kleiner Schmelzsicherungen programmierbar. | Alte Industrie-Steuerungen |
| | **EPROM** *(veraltet)* | *Erasable PROM*: Durch starkes **UV-Licht** über ein rundes Quarzglasfenster auf dem Chip komplett löschbar. | Alte BIOS-Chips (80er/90er Jahre) |
| | **EEPROM & Flash** | *Electrically Erasable PROM*: **Rein elektrisch** in Millisekunden lösch- und wiederbeschreibbar (Flash löscht blockweise). | **Moderne UEFI/BIOS-Chips**, **SSDs**, **USB-Sticks**, SD-Karten |
| **Flüchtig** *(Daten sind ohne Strom sofort weg)* | **SRAM** (*Static RAM*) | Speichert 1 Bit in einem **Flip-Flop aus 6 Transistoren**. Hält die Daten stabil ohne Refresh, solange Strom anliegt. | **CPU-Register & CPU-Cache (L1, L2, L3)** |
| | **DRAM** (*Dynamic RAM*) | Speichert 1 Bit in **1 winzigen Kondensator + 1 Transistor**. Kondensator entlädt sich durch Leckströme $\rightarrow$ braucht **ständigen Refresh** (alle paar ms)! | **Arbeitsspeicher / Hauptspeicher** (SDRAM, DDR4, DDR5) |
| | **CMOS-RAM** | Extrem sparsamer RAM für Datum/Uhrzeit und BIOS-Settings; wird bei ausgeschaltetem PC durch die **CR2032-Mainboard-Batterie** gepuffert. | Echtzeituhr (RTC) / Legacy-BIOS-Konfiguration |

---

### 2. Direktvergleich: SRAM vs. DRAM (Wichtige KA-Tabelle!)

| Merkmal | SRAM (*Static RAM*) | DRAM (*Dynamic RAM*) |
| :--- | :--- | :--- |
| **Bauteile pro Bit** | **6 Transistoren** (Flip-Flop-Schaltung) | **1 Kondensator + 1 Transistor** |
| **Refresh-Zyklus nötig?** | **Nein** (statisch stabil, solange Spannung anliegt) | **Ja, zwingend periodisch** (dynamisch, da Kondensator Ladung verliert) |
| **Zugriffszeit (Tempo)** | **Extrem schnell** ($\approx 1\text{–}3\text{ ns}$) | **Deutlich langsamer** ($\approx 50\text{–}80\text{ ns}$) |
| **Platzbedarf & Preis pro GB** | **Riesig & extrem teuer** (geringe Packungsdichte) | **Winzig & sehr günstig** (sehr hohe Packungsdichte) |

---

### 3. SDRAM vs. DDR-SDRAM im Vergleich (inklusive Impulsdiagramm!)

* **SDR-SDRAM (*Single Data Rate Synchronous DRAM*):** Synchronisiert sich mit dem Systemtakt, überträgt aber **nur bei der steigenden Taktflanke (Wechsel von 0 auf 1)** genau **1 Datenwort pro Taktzyklus**.
* **DDR-SDRAM (*Double Data Rate* – DDR1 bis DDR5):** Nutzt **sowohl die steigende Taktflanke ALS AUCH die fallende Taktflanke** zur Datenübertragung! Dadurch werden pro Taktzyklus **2 Datenworte** übertragen $\rightarrow$ **doppelte Datenrate bei exakt gleicher Taktfrequenz**!

```text
IMPULSDIAGRAMM: SDR-SDRAM vs. DDR-SDRAM

              Steigend   Fallend    Steigend   Fallend
                 ↑          ↓          ↑          ↓
                 |          |          |          |
           +-----+          +----------+          +-----
Taktsignal |     |          |          |          |
(Clock)    +     +----------+          +----------+
           |<--- 1 Taktzyklus --->|<--- 1 Taktzyklus --->|

SDR-SDRAM: [   Daten 0   ]        [   Daten 1   ]
(1x/Takt)    (nur bei ↑)            (nur bei ↑)

DDR-SDRAM: [Daten 0][Daten 1]     [Daten 2][Daten 3]
(2x/Takt)   (bei ↑)  (bei ↓)       (bei ↑)  (bei ↓)
```

> **Zusatzwissen für Experten (Generationen DDR1 bis DDR5):**  
> Jede neue DDR-Generation erhöht den internen *Prefetch-Puffer* (DDR1 = 2n, DDR2 = 4n, DDR3 = 8n, DDR4 = 8n mit Bankgruppen, DDR5 = 16n mit zwei unabhängigen 32-Bit-Subkanälen pro Riegel) und **senkt gleichzeitig die Betriebsspannung** (DDR1: $2{,}5\text{ V}$ $\rightarrow$ DDR3: $1{,}5\text{ V}$ $\rightarrow$ DDR4: $1{,}2\text{ V}$ $\rightarrow$ DDR5: **$1{,}1\text{ V}$**). Zudem haben alle Generationen die **Kerbe an einer anderen Position**, damit man niemals versehentlich DDR4 in einen DDR5-Slot stecken kann!

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Auf der Verpackung eines Arbeitsspeichers steht ‚DDR4-3200‘. Ein Überwachungstool (wie CPU-Z) zeigt unter Windows jedoch eine reale Taktfrequenz von nur 1.600 MHz an. Wurde der Kunde betrogen oder arbeitet der RAM fehlerhaft?“*
   * **Lösung:** Weder noch – alles ist völlig korrekt! Durch das **DDR-Verfahren (*Double Data Rate*)** überträgt der Speicher bei einem realen I/O-Takt von **$1.600\text{ MHz}$** sowohl bei der steigenden als auch bei der fallenden Taktflanke Daten. Er erreicht damit exakt dieselbe effektive Übertragungsrate wie ein alter SDR-Speicher mit $3.200\text{ MHz}$ (korrekte Einheit: **$3.200\text{ MT/s}$** = *Megatransfers pro Sekunde*).
2. **Frage 2:** *„Ein älterer Büro-PC verliert jedes Mal, wenn man abends die schaltbare Steckdosenleiste ausschaltet, das aktuelle Datum, die Uhrzeit und seine Boot-Reihenfolge. Welcher Speicherbaustein ist betroffen und warum verliert er plötzlich seine Daten?“*
   * **Lösung:** Betroffen ist das **CMOS-RAM** (bzw. die Echtzeituhr RTC), das ein **flüchtiger Speicher** ist. Normalerweise hält die runde **CR2032-Knopfzelle (Mainboard-Batterie)** das CMOS-RAM unter Spannung, wenn der PC vom Stromnetz getrennt wird. Ist diese Batterie nach einigen Jahren leer, gehen alle Einstellungen verloren, sobald die Steckdosenleiste ausgeschaltet wird.
