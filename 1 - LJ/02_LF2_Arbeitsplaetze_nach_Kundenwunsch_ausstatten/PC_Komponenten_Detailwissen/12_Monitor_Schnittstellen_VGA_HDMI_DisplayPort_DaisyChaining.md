# 🖥️ LS 1 – Thema 12: Monitor-Schnittstellen (VGA, DisplayPort, HDMI inkl. Pinbelegung & Daisy Chaining)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten  
> **AP1-Statistik:** *Monitoranschlüsse & Daisy Chaining* brachten in den AP1-Prüfungen (2022F, 2024F, 2026F) zusammen **13 Punkte**!

---

## ✅ Lehrinhalte nach LS 1 & AP1
- [ ] Technischer Vergleich: **VGA vs. HDMI vs. DisplayPort** (inkl. USB-C DP Alt Mode)
- [ ] **HDMI-Pinbelegung (19 Pins):** Aufbau & Funktionsgruppen erklären
- [ ] **Daisy Chaining (Reihenschaltung von Monitoren)** über DisplayPort MST / Thunderbolt

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Direktvergleich: VGA vs. HDMI vs. DisplayPort

| Merkmal | VGA (15-polig D-Sub, blau) | HDMI (19-polig Typ A) | DisplayPort (20-polig) |
| :--- | :--- | :--- | :--- |
| **Signalübertragung** | **Rein analog** (asymmetrische RGB-Spannungen $0\text{–}0{,}7\text{ V}$ + HSync/VSync). | **Rein digital** (symmetrisch/differenziell über TMDS bzw. FRL). | **Rein digital & paketbasiert** (Mikropakete wie bei Ethernet/PCIe!). |
| **Audio-Übertragung?** | **Nein!** (Nur Bild, separates Audiokabel nötig). | **Ja!** (Bild + unkomprimiertes Mehrkanal-Audio + Ethernet + CEC). | **Ja!** (Bild + Mehrkanal-Audio). |
| **Stecker-Sicherung** | Zwei seitliche **Rändelschrauben**. | Keine Verriegelung (nur gesteckt). | **Mechanische Rastnasen** mit Entriegelungsknopf am Stecker (verhindert Herausfallen!). |
| **Daisy Chaining (Mehrere Monitore an 1 Kabel)?** | **Nein** | **Nein** | **Ja!** (Über **MST – *Multi-Stream Transport*** ab DP 1.2). |
| **Max. Datenrate & Auflösung** | Veraltet (ab Full HD $1920 \times 1080$ unscharfes/verwaschenes Bild bei langen Kabeln). | HDMI 2.0: $18\text{ Gbit/s}$ (4K @ 60 Hz)<br>HDMI 2.1: **$48\text{ Gbit/s}$** (4K @ 120/144 Hz, 8K) | DP 1.4: $32{,}4\text{ Gbit/s}$<br>DP 2.1: bis **$80\text{ Gbit/s}$** |

---

### 2. HDMI-Pinbelegung (19 Pins – Logische Funktionsgruppen)

```text
  Pin 1   Pin 3   Pin 5   Pin 7   Pin 9   Pin 11  Pin 13  Pin 15  Pin 17  Pin 19
 +-------------------------------------------------------------------------------+
 |  [1]     [3]     [5]     [7]     [9]    [11]    [13]    [15]    [17]    [19]  |
  \    [2]     [4]     [6]     [8]    [10]    [12]    [14]    [16]    [18]      /
   +---------------------------------------------------------------------------+
     Pin 2   Pin 4   Pin 6   Pin 8   Pin 10  Pin 12  Pin 14  Pin 16  Pin 18
```

Die **19 Pins eines Standard-HDMI-Steckers (Typ A)** gliedern sich in **5 logische Gruppen**:
1. **Pins 1 bis 9 – Die 3 TMDS-Datenkanäle (*Transition Minimized Differential Signaling*):**
   * **Kanal 2 (Pins 1–3):** `TMDS Data2+`, `Data2 Shield (Masse)`, `TMDS Data2-` (überträgt primär **Rot** + Audio/Steuerdaten)
   * **Kanal 1 (Pins 4–6):** `TMDS Data1+`, `Data1 Shield`, `TMDS Data1-` (überträgt primär **Grün**)
   * **Kanal 0 (Pins 7–9):** `TMDS Data0+`, `Data0 Shield`, `TMDS Data0-` (überträgt primär **Blau**)
   * *(Jeder Kanal nutzt ein **differenzielles Adernpaar `+` und `-`** mit eigener Schirmung gegen Störungen!)*
2. **Pins 10 bis 12 – Der TMDS-Taktkanal (*Clock Channel*):**
   * `TMDS Clock+` (Pin 10), `Clock Shield` (Pin 11), `TMDS Clock-` (Pin 12) zur exakten zeitlichen Synchronisation des Empfängers.
3. **Pin 13 & Pin 14 – Steuerung & Zusatzfunktionen:**
   * **Pin 13 (`CEC` – *Consumer Electronics Control*):** Erlaubt die Steuerung mehrerer Geräte mit nur einer Fernbedienung.
   * **Pin 14 (`Reserved / HEC + eARC`):** Ab HDMI 1.4/2.1 genutzt für den **Audio Return Channel (ARC/eARC)** (Ton vom TV zurück zur Soundbar) und HDMI-Ethernet-Kanal.
4. **Pins 15 bis 18 – DDC-Kommunikation (*Display Data Channel*) & 5V-Versorgung:**
   * **Pin 15 (`SCL`) & Pin 16 (`SDA`):** Serieller **I²C-Bus**, über den die Grafikkarte beim Anschließen den **EDID-Speicherchip** des Monitors ausliest (*Modellname, Seriennummer, unterstützte Auflösungen und Bildwiederholraten*) und den Kopierschutz **HDCP** aushandelt!
   * **Pin 17 (`DDC/CEC Ground`)** & **Pin 18 (`+5 V Power`, max. 50 mA):** Versorgt den EDID-Chip im Monitor mit Strom, sodass der PC den Monitor auch dann schon erkennt, wenn der Monitor noch im Standby ist!
5. **Pin 19 – `Hot Plug Detect (HPD)`:**
   * Meldet der Grafikkarte sofort elektrisch, sobald ein Monitor im laufenden Betrieb angesteckt oder abgezogen wird.

---

### 3. Daisy Chaining (Reihenschaltung von Monitoren – AP1 Frühjahr 2026!)
* **Prinzip:** Statt von der Grafikkarte zu jedem Monitor ein eigenes langes Kabel zu ziehen (Sternverkabelung), verbindet man den **PC nur mit Monitor 1** und schaltet **Monitor 2 direkt per kurzem Kabel an den Ausgang von Monitor 1** (`PC ---> [DP In] Monitor 1 [DP Out] ---> [DP In] Monitor 2`).
* **3 technische Voraussetzungen:**
  1. Nutzung von **DisplayPort (ab Version 1.2)** oder **USB-C / Thunderbolt** *(funktioniert NICHT mit HDMI oder VGA!)*.
  2. Monitor 1 muss zwingend einen **DisplayPort-Ausgang (`DP Out`)** besitzen und **MST (*Multi-Stream Transport*)** im Monitor-Menü aktiviert haben.
  3. Die Gesamtbandbreite des Kabels vom PC zu Monitor 1 muss für die Summe aller Monitore ausreichen.

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Warum wird das Bild bei einem alten analogen VGA-Kabel mit zunehmender Kabellänge (z. B. 10 m zum Beamer) und hoher Auflösung sichtbar unscharf und wirft Schatten, während das Bild bei einem digitalen HDMI- oder DisplayPort-Kabel entweder zu 100 % gestochen scharf ist oder komplett ausfällt?“*
   * **Lösung:** Bei **VGA** wird die Helligkeit jedes Pixels als kontinuierliche **analoge Spannung zwischen $0\text{ V}$ und $0{,}7\text{ V}$** übertragen. Jede Dämpfung, Reflexion oder elektromagnetische Störung auf einem langen Kabel verändert diese Spannung minimal, was direkt als Unschärfe, Farbverfälschung oder Geisterbild sichtbar wird. **HDMI und DisplayPort** übertragen dagegen **digitale Zustände (`0` und `1`) als differenzielle Signale**: Solange der Empfänger eine `0` noch von einer `1` unterscheiden kann, ist das Bild **verlustfrei bis aufs letzte Pixel perfekt** (erst wenn das Kabel viel zu lang ist und die Bitfehlerrate kippt, reißt das Signal plötzlich komplett ab – *Cliff Effect*).
2. **Frage 2:** *„Welche Rolle spielen die Pins 15, 16 und 18 (DDC + 5V) beim Einstecken eines HDMI-Kabels, noch bevor überhaupt das erste Fernsehbild übertragen wird?“*
   * **Lösung:** Über Pin 18 (`+5 V`) versorgt die Grafikkarte den kleinen **EDID-Speicherchip (EEPROM)** im Monitor mit Strom und liest anschließend über den **DDC-Kanal (Pins 15 `SCL` und 16 `SDA`)** die technischen Fähigkeiten des Monitors aus (native Auflösung, maximale Bildwiederholrate in Hz, unterstützte Audioformate). Nur dadurch weiß Windows automatisch, dass es z. B. ein Bild in $2560 \times 1440$ bei $144\text{ Hz}$ senden soll!
