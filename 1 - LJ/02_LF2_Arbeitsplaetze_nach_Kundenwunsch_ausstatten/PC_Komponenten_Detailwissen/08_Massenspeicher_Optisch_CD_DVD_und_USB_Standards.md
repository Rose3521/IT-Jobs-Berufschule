# 💿 LS 1 – Thema 8: Massenspeicher mit Controller (CD/DVD & USB 1.1 bis USB4 im Vergleich)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] **Optische Speicher (CD, DVD, Blu-ray):** Aufbau, Pits & Lands, Laser-Abtastung
- [ ] **USB-Standards (1.1, 2.0, 3.0/3.2 und USB4):** Tabellarischer Vergleich, Funktionsprinzipien, Aufbau & Technik

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Optische Massenspeicher (CD, DVD, Blu-ray) – Funktionsprinzip
* **Aufbau:** Eine Kunststoffscheibe (Polycarbonat) enthält eine einzige, von innen nach außen verlaufende **Spiralspur** mit mikroskopisch kleinen **Vertiefungen (*Pits*)** und **Flächen (*Lands*)**, die mit einer reflektierenden Aluminiumschicht bedampft ist.
* **Wie werden `0` und `1` erkannt? (Achtung Prüfungsfalle!):**
  * Ein Laserstrahl wird von unten auf die Spur gerichtet und eine Fotodiode misst das reflektierte Licht.
  * Trifft der Laser voll auf ein *Land* oder voll in ein *Pit*, bleibt die Helligkeit konstant (**= `0`**).
  * Trifft der Laser genau auf den **Übergang (die Stufenkante!) zwischen Pit und Land**, wird ein Teil des Lichts vom Land und ein Teil vom Pit reflektiert. Da das Pit exakt $\frac{1}{4}$ der Laser-Wellenlänge tief ist, legt das Licht aus dem Pit den halben Wellenlängenweg ($\frac{1}{2} \lambda$) mehr zurück $\rightarrow$ **destruktive Interferenz (Auslöschung)**! Dieser Helligkeitswechsel an der Kante wird als **`1`** gewertet!
* **Warum steigt die Kapazität von CD $\rightarrow$ DVD $\rightarrow$ Blu-ray so stark an?**
  * Durch **immer kürzere Wellenlängen des Lasers** (kleinerer Fokus-Punkt = viel kleinere Pits und engerer Spurabstand!):
    * **CD ($700\text{ MB}$):** Infrarot-Laser ($780\text{ nm}$)
    * **DVD ($4{,}7\text{ GB}$ Single-Layer / $8{,}5\text{ GB}$ Dual-Layer):** Roter Laser ($650\text{ nm}$)
    * **Blu-ray ($25\text{ GB}$ Single-Layer / $50\text{ GB}$ Dual-Layer):** Violett-blauer Laser (**$405\text{ nm}$**)

---

### 2. USB (Universal Serial Bus) – Aufbau, Technik & Generationen im Vergleich

* **Technisches Prinzip von USB:**
  * **Host-zentrierter, serieller Bus (Baumtopologie über Hubs):** Es gibt immer genau einen **Host-Controller** (im PC), der alle angeschlossenen Endgeräte (**Functions/Devices**, bis zu 127 Geräte pro Controller) per **Polling** verwaltet.
  * **Hot-Plugging & Plug-and-Play:** Geräte können im laufenden Betrieb ein- und ausgesteckt werden (die beiden äußeren Stromkontakte im Stecker sind minimal länger als die beiden inneren Datenkontakte, sodass das Gerät **zuerst mit Strom versorgt wird, bevor der Datenkontakt schließt**!).

| USB-Generation | Offizieller Name (heute) | Max. Brutto-Datenrate | Adern / Kontakte & Duplex-Modus | Max. Ladeleistung |
| :--- | :--- | :---: | :--- | :--- |
| **USB 1.1** | *Full Speed* | **$12\text{ Mbit/s}$** ($\approx 1{,}5\text{ MB/s}$) | **4 Kontakte** (`+5V`, `D-`, `D+`, `GND`) – **Halb-Duplex** | $5\text{ V} \cdot 0{,}5\text{ A} = \mathbf{2{,}5\text{ W}}$ |
| **USB 2.0** | *High Speed* | **$480\text{ Mbit/s}$** ($\approx 40\text{–}60\text{ MB/s}$) | **4 Kontakte** (`+5V`, `D-`, `D+`, `GND`) – **Halb-Duplex** | $5\text{ V} \cdot 0{,}5\text{ A} = \mathbf{2{,}5\text{ W}}$ |
| **USB 3.0** | *USB 3.2 Gen 1 (SuperSpeed)* | **$5\text{ Gbit/s}$** ($\approx 450\text{ MB/s}$) | **9 Kontakte** im blauen Typ-A-Stecker (4 alte Kontakte + **5 neue Kontakte** für getrenntes Senden `SSTX` & Empfangen `SSRX` $\rightarrow$ **Voll-Duplex**!) | $5\text{ V} \cdot 0{,}9\text{ A} = \mathbf{4{,}5\text{ W}}$ |
| **USB 3.1** | *USB 3.2 Gen 2 (SuperSpeed+)* | **$10\text{ Gbit/s}$** ($\approx 1.000\text{ MB/s}$) | 9 Kontakte (Typ-A) bzw. **24 Kontakte im verdrehsicheren USB-C-Stecker** – **Voll-Duplex** | Bis $100\text{ W}$ (mit USB Power Delivery über Typ-C) |
| **USB 3.2** | *USB 3.2 Gen 2x2* | **$20\text{ Gbit/s}$** ($\approx 2.000\text{ MB/s}$) | Nutzt **2 Lanes à 10 Gbit/s gleichzeitig** $\rightarrow$ funktioniert **NUR noch über USB-Typ-C-Kabel**! | Bis $100\text{ W}$ (USB-PD) |
| **USB4** | *USB4 40Gbps (bzw. v2.0 80Gbps)* | **$40\text{ Gbit/s}$** (bzw. $80\text{ Gbit/s}$) | **Nur USB-Typ-C**; basiert auf **Thunderbolt-3/4-Technik** und kann **PCIe-, DisplayPort- und USB-Daten gleichzeitig über ein Kabel tunneln**! | Bis **$240\text{ W}$** ($48\text{ V} \cdot 5\text{ A}$ USB-PD 3.1 EPR) |

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Warum hat ein USB-Typ-C-Stecker zwar die exakt gleiche Steckerform wie ein günstiges Smartphone-Ladekabel, erreicht aber manchmal nur langsame 480 Mbit/s (USB 2.0) und kein Bildsignal für einen Monitor?“*
   * **Lösung:** **„USB-C“ bezeichnet ausschließlich die physische Steckerform (24 Pins, verdrehsicher), sagt aber absolut nichts über das darin verwendete elektrische Protokoll oder die belegten Adern im Kabel aus!** In billigen USB-C-Ladekabeln sind im Inneren neben den Stromleitungen nur die 2 alten `D+/D-`-Drähte für **USB 2.0 ($480\text{ Mbit/s}$)** verlötet, während die 4 High-Speed-Adernpaare für USB 3.2/USB4 und den *DisplayPort Alternate Mode* komplett fehlen.
2. **Frage 2:** *„Warum wurden bei USB 3.0 im bekannten rechteckigen Typ-A-Stecker 5 zusätzliche Kontakte hinten im Stecker ergänzt (insgesamt 9 Kontakte), statt einfach die vorhandenen 4 Kontakte von USB 2.0 schneller zu takten?“*
   * **Lösung:** USB 2.0 besitzt für Daten nur **ein einziges Adernpaar (`D+` / `D-`)** und kann deshalb nur im **Halb-Duplex** (abwechselnd Senden oder Empfangen) arbeiten. Für die hohe Geschwindigkeit von USB 3.0 ($5\text{ Gbit/s}$) war zwingend **Voll-Duplex** (gleichzeitiges Senden und Empfangen) erforderlich, wofür zwei zusätzliche abgeschirmte Adernpaare (`SSTX+/SSTX-` und `SSRX+/SSRX-` plus Signalmasse = 5 Pins) benötigt wurden – während die alten 4 Pins vorne im Stecker für die **Abwärtskompatibilität zu USB 2.0** erhalten blieben.
