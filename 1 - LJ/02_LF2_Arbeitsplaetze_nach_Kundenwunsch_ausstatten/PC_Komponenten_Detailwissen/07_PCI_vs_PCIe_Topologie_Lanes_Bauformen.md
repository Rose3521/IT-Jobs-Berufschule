# ⚡ LS 1 – Thema 7: PCI vs. PCIe (Topologie, Lanes, Bauformen & Kompatibilität)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] Technik & Architektur/Topologie: **Bus (PCI) vs. Stern / Punkt-zu-Punkt (PCIe)**
- [ ] Betriebsmodus: Parallel/Halb-Duplex vs. Seriell/Voll-Duplex
- [ ] Aufbau einer **Lane**, physikalische Bauformen (`x1`, `x4`, `x8`, `x16`) & Anbindung der Grafikkarte
- [ ] Mechanische und elektrische **Kompatibilität** (Aufwärts-/Abwärtskompatibilität)

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Architektur-Vergleich: Paralleler Bus (PCI) vs. Serieller Punkt-zu-Punkt-Stern (PCIe)

```text
KLASSISCHES PCI (GETEILTER PARALLELER BUS / SHARED BUS):
+-----+
| CPU | ===[ Gemeinsamer 32-Bit Parallelbus (Halb-Duplex, max. 133 MB/s für ALLE zusammen!) ]===
+-----+          |                       |                       |
            [PCI Slot 1]            [PCI Slot 2]            [PCI Slot 3]
         (Wenn Slot 1 sendet, müssen Slot 2 und Slot 3 warten!)


MODERNES PCIe (SERIELLE PUNKT-ZU-PUNKT-VERBINDUNGEN / STERNTOPOLOGIE):
                 +----------------------------------+
                 |   CPU / PCIe Root Complex / Switch|
                 +----------------------------------+
                   /               |                \
    (16 eigene Lanes)       (4 eigene Lanes)      (1 eigene Lane)
      Voll-Duplex!            Voll-Duplex!         Voll-Duplex!
          /                        |                     \
  [PCIe x16 Grafikkarte]    [M.2 NVMe SSD x4]     [PCIe x1 Netzwerkkarte]
  (Jedes Gerät hat seine eigene exklusive Autobahn und bremst kein anderes aus!)
```

| Merkmal | Klassisches PCI | Modernes PCIe (*PCI Express*) |
| :--- | :--- | :--- |
| **Topologie** | **Geteilter Bus (*Shared Bus*)**: Alle Steckplätze teilen sich dieselben Leitungen und dieselbe Gesamtbandbreite ($133\text{ MB/s}$). | **Sterntopologie (Punkt-zu-Punkt)** über den *Root Complex / PCIe-Switch*: Jeder Slot hat seine eigenen dedizierten Leitungen (**Lanes**). |
| **Übertragungsart** | **Parallel** (32 Bit gleichzeitig nebeneinander) & **Halb-Duplex** (nur Senden *oder* Empfangen zur selben Zeit). | **Seriell** pro Lane & **Voll-Duplex (*Full-Duplex*)** (gleichzeitiges Senden *und* Empfangen mit voller Geschwindigkeit!). |
| **Physikalisches Limit** | **Clock Skew (Laufzeitversatz):** Bei hohen Frequenzen kommen die 32 parallelen Bits durch minimal unterschiedlich lange Leiterbahnen nicht mehr gleichzeitig an! | Kein Laufzeitversatz zwischen Datenbits, da pro Lane seriell mit eingebettetem Taktsignal übertragen wird $\rightarrow$ extrem hohe Gigahertz-Taktraten möglich! |

---

### 2. Was ist eine PCIe-Lane und welche Bauformen gibt es?
* **Aufbau von 1 PCIe-Lane (`x1`):**
  * Besteht aus **4 einzelnen Kupferleitungen** (2 differenzielle Adernpaare):
    * **1 Adernpaar (`TX+` / `TX-`) zum Senden**
    * **1 Adernpaar (`RX+` / `RX-`) zum Empfangen**
* **Die 4 physikalischen Slot-Bauformen:**
  * **`PCIe x1` (1 Lane, 18 Pins, ganz kurz):** Für Soundkarten, WLAN-Karten, 1/2,5-Gbit-Netzwerkkarten.
  * **`PCIe x4` (4 Lanes, 32 Pins):** Für M.2-NVMe-SSDs, 10-Gbit-Netzwerkkarten, RAID-Controller.
  * **`PCIe x8` (8 Lanes, 49 Pins):** Für große Server-RAID-/Fibre-Channel-Controller.
  * **`PCIe x16` (16 Lanes, 89 Pins, ganz lang):** Für **dedizierte Grafikkarten (GPUs)** (direkt an die CPU angebunden!).

### 3. Geschwindigkeiten & Kompatibilitätsregeln
| Generation | Netto-Tempo pro Lane (`x1`) | Netto-Tempo für M.2 SSD (`x4`) | Netto-Tempo für Grafikkarte (`x16`) |
| :---: | :---: | :---: | :---: |
| **PCIe 3.0** | $\approx \mathbf{1\text{ GB/s}}$ | $\approx 3{,}9\text{ GB/s}$ | $\approx 15{,}8\text{ GB/s}$ |
| **PCIe 4.0** | $\approx \mathbf{2\text{ GB/s}}$ | $\approx 7{,}9\text{ GB/s}$ | $\approx 31{,}5\text{ GB/s}$ |
| **PCIe 5.0** | $\approx \mathbf{4\text{ GB/s}}$ | $\approx 15{,}8\text{ GB/s}$ | $\approx 63{,}0\text{ GB/s}$ |

* **Regel 1 (Slot-Größe):** Man kann **jede kleinere PCIe-Karte (z. B. `x1` oder `x4`) problemlos in einen größeren PCIe-Slot (z. B. `x16`) stecken**! Die Karte nutzt dann einfach nur die vorderen Kontakte des großen Slots.
* **Regel 2 (Generationen):** PCIe ist **vollständig auf- und abwärtskompatibel** (PCIe 1.0 bis 5.0). Eine PCIe-4.0-Grafikkarte läuft auch in einem PCIe-3.0-Mainboard – das Tempo wird automatisch auf die niedrigere gemeinsame Generation (PCIe 3.0) ausgehandelt.

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Auf einem ATX-Mainboard befinden sich zwei lange `PCIe x16`-Steckplätze. Im Handbuch steht jedoch beim unteren Slot: ‚PCIEX4 (mechanisch x16, elektrisch x4)‘. Was bedeutet das und was passiert, wenn man die High-End-Grafikkarte versehentlich in den unteren statt in den oberen Slot steckt?“*
   * **Lösung:** Das Gehäuse des unteren Steckplatzes ist zwar äußerlich genauso lang wie ein `x16`-Slot (damit lange Karten mechanisch hineinpassen), im Inneren sind aber **nur die vorderen 4 Lanes (`x4`) elektrisch mit dem Chipsatz verlötet**, während die hinteren 12 Lanes leer sind! Steckt man die Grafikkarte in diesen unteren Slot, erhält sie statt 16 direkten CPU-Lanes nur **ein Viertel der Bandbreite (4 Lanes über den langsameren Chipsatz)** und wird bei grafikintensiven Anwendungen spürbar ausgebremst.
2. **Frage 2:** *„Wie viele einzelne Datenleitungen (Drähte/Leiterbahnen) benötigt ein `PCIe x16`-Slot allein für die Datenübertragung der 16 Lanes (ohne Strom- und Massepins)?“*
   * **Lösung:** Jede einzelne PCIe-Lane besteht für den Voll-Duplex-Betrieb aus **4 differentiellen Datenleitungen** (2 für Senden `TX+/TX-`, 2 für Empfangen `RX+/RX-`). Bei 16 Lanes sind das $16 \times 4 = \mathbf{64\text{ Datenleitungen}}$.
