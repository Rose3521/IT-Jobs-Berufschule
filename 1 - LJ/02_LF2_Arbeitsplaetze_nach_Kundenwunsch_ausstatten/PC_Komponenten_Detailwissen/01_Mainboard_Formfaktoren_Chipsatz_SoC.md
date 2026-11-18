# 🧩 LS 1 – Thema 1: Mainboard (Eigenschaften, Formfaktoren, Chipsatz & SoC)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte according to LS 1
- [ ] Eigenschaften, Aufgaben & Leistungsmerkmale eines Mainboards
- [ ] Formfaktoren & Abmessungen (E-ATX, ATX, Micro-ATX, Mini-ITX)
- [ ] Chipsatz-Architektur (Northbridge-Integration in CPU vs. PCH) & Kompatibilität
- [ ] Vergleich: **PC-Mainboard vs. Server-Mainboard vs. Smartphone-Mainboard (SoC)**

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Aufgaben & Moderne Chipsatz-Architektur (PCH)
* Das **Mainboard (*Motherboard* / Hauptplatine)** verbindet alle Systemkomponenten mechanisch (Sockel, Steckplätze) und elektrisch (Daten-, Adress- und Steuerleitungen).
* **Evolution des Chipsatzes:**
  * *Klassisch:* **Northbridge** (bandbreitenkritische Anbindung von RAM und Grafikkarte an die CPU) + **Southbridge** (langsame I/O wie USB, SATA, Audio, BIOS).
  * *Heute:* Die **Northbridge sitzt komplett in der CPU!** Der Speichercontroller (**IMC** für DDR4/DDR5) sowie die primären **PCIe-Lanes** (für die Grafikkarte `x16` und die erste M.2-NVMe-SSD `x4`) sind direkt in den Prozessor integriert, um Latenzen zu minimieren.
  * Auf dem Mainboard verbleibt nur noch ein einziger Chip: der **PCH (*Platform Controller Hub* / Chipsatz)**, der über **DMI** (gebündelte PCIe-Lanes) mit der CPU kommuniziert und SATA, USB, LAN, Onboard-Audio sowie weitere PCIe-/M.2-Slots bereitstellt.

### 2. Formfaktoren & Abmessungen (Kompatibilität zum Gehäuse)
* **E-ATX (*Extended ATX*):** **$305 \times 330\text{ mm}$** – für High-End-Workstations und Server (oft 8 RAM-Bänke, viele PCIe-Slots).
* **ATX (*Standard*):** **$305 \times 244\text{ mm}$** – bis zu 7 Erweiterungsslots, meist 4 RAM-Bänke.
* **Micro-ATX ($\mu\text{ATX}$):** **$244 \times 244\text{ mm}$** – quadratisch, bis zu 4 Erweiterungsslots; **abwärtskompatibel** zu den Schraubabständen im großen ATX-Gehäuse!
* **Mini-ITX:** **$170 \times 170\text{ mm}$** – extrem kompakt für Mini-Büro-PCs; besitzt nur **1 einzigen PCIe-Steckplatz** und 2 RAM-Bänke.

### 3. Vergleich: PC-Mainboard vs. Server-Mainboard vs. Smartphone-Mainboard

| Kriterium | Desktop-PC-Mainboard | Server-Mainboard | Smartphone-Mainboard (Logic Board) |
| :--- | :--- | :--- | :--- |
| **Aufbau & Modularität** | **Modular:** Gesockelte CPU (LGA/AM5), gesteckter RAM (DIMM), PCIe-Slots für Grafikkarte. | **Maximal skalierbar & redundant:** Oft **Dual-Socket** (2 CPUs), 8–24 RAM-Slots (**ECC RDIMM**), viele PCIe-Lanes, Hot-Swap-Lüfteranschlüsse. | **Hochintegriertes SoC (*System on a Chip*):** CPU, GPU, NPU, RAM (*Package-on-Package*), Modem und WLAN sitzen **in einem einzigen Chip**! Keine Aufrüstbarkeit. |
| **Fernwartung (Out-of-Band)** | Keine (maximal *Wake-on-LAN*). | Eigener **BMC-Chip (IPMI / Dell iDRAC / HP iLO)** mit separatem RJ45-Management-Port: Volle Kontrolle (Ein-/Ausschalten, BIOS-Zugriff, ISO-Mount) über Browser, selbst wenn der Server ausgeschaltet oder mit Bluescreen abgestürzt ist! | Keine Hardware-Fernwartung (Verwaltung über MDM im Betriebssystem). |
| **Fokus** | Preis-Leistung, Audio/RGB, USB-Vielfalt, Gaming/Office. | **$24/7$-Dauerbetrieb**, Ausfallsicherheit, ECC-Unterstützung, gerader Luftstrom für 19"-Racks. | Minimaler Platzbedarf, extrem geringe Leistungsaufnahme ($<5\text{–}10\text{ W}$), komplett lüfterlos. |

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1 (Lane-Sharing):** *„Ein Kunde rüstet auf seinem PC-Mainboard im zweiten M.2-Slot eine NVMe-SSD nach. Direkt danach verschwinden zwei seiner vier SATA-Festplatten aus dem BIOS und aus Windows. Erklären Sie technisch anhand der Chipsatz-Architektur, warum kein Defekt vorliegt!“*
   * **Lösung:** Der Mainboard-Chipsatz (PCH) verfügt nur über eine begrenzte Anzahl an High-Speed-I/O-Lanes (*HSIO-Lanes*). Daher nutzen viele Mainboards **Lane-Sharing**: Wird der zweite M.2-Slot mit einer PCIe-x4-SSD belegt, schaltet der Chipsatz die physischen Leitungen von zwei SATA-Ports auf den M.2-Slot um, wodurch diese beiden SATA-Buchsen deaktiviert werden. Lösung: SATA-Kabel auf die verbleibenden, nicht geteilten SATA-Ports umstecken.
2. **Frage 2 (Server vs. PC):** *„Warum verzichtet ein Server-Mainboard fast immer auf eine starke Onboard-Soundkarte und schnelle Gaming-Videoausgänge, besitzt dafür aber einen winzigen 2D-Grafikchip (z. B. ASPEED) mit VGA-Anschluss und einen eigenen Netzwerkport nur für diesen Chip?“*
   * **Lösung:** Ein Server steht im abgeschlossenen Serverraum/Rechenzentrum, wo niemand Musik hört oder 3D-Spiele spielt. Der kleine ASPEED-Chip ist der **BMC (*Baseboard Management Controller* für IPMI/iDRAC/iLO)**: Er arbeitet völlig unabhängig von der Haupt-CPU mit minimalem Stromverbrauch und leitet das Bildsignal über den dedizierten Management-Netzwerkport an den Administrator im Büro weiter (*Remote-KVM*), sodass der Server aus der Ferne gewartet werden kann.
