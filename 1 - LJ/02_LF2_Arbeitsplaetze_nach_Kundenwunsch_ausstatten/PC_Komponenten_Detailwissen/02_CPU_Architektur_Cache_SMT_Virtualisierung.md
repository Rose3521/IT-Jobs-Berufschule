# 🧠 LS 1 – Thema 2: CPU (Bitsystem, Takt, Kerne/SMT, Cache, Virtualisierung & Stromverbrauch)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] **Bitsystem** (32-Bit vs. 64-Bit-Architektur & Adressraum)
- [ ] **Taktfrequenz**, **IPC** & **Anzahl der Kerne**
- [ ] **Cache-Größe und Levels** (L1, L2, L3)
- [ ] **Multi-Threading (SMT / Hyper-Threading)** & **Virtualisierungs-Unterstützung (VT-x / AMD-V)**
- [ ] **Stromverbrauch** bei verschiedenen Leistungsstufen (TDP, P-States, C-States)

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Von-Neumann-Prinzip & Bitsystem (32-Bit vs. 64-Bit)
* Die **CPU (*Central Processing Unit*)** besteht im Kern aus dem **Rechenwerk (ALU)**, dem **Steuerwerk (Control Unit)**, extrem schnellen **Registern** und dem **Cache**.
* **32-Bit vs. 64-Bit:**
  * Beschreibt die Breite der internen CPU-Register und die Anzahl der Adressleitungen.
  * **32-Bit-CPU:** Kann maximal $2^{32}\text{ Byte} = 4.294.967.296\text{ Byte} = \mathbf{4\text{ GiB}}$ Arbeitsspeicher direkt adressieren! (Baut man 16 GB RAM ein, kann ein 32-Bit-System trotzdem nur knapp 4 GB nutzen!).
  * **64-Bit-CPU (x86-64 / AMD64, ARM64):** Kann theoretisch $2^{64}\text{ Byte} = \mathbf{16\text{ Exbibyte}}$ adressieren und rechnet mit doppelt so breiten 64-Bit-Datenworten pro Takt.

### 2. Taktfrequenz, Kerne & Multi-Threading (SMT / Hyper-Threading)
* **Taktfrequenz ($\text{GHz}$):** Gibt an, wie viele Milliarden Schalttakte pro Sekunde ausgeführt werden. Da verschiedene CPU-Architekturen unterschiedlich viele Befehle pro Takt schaffen (**IPC – *Instructions Per Cycle***), darf man die Taktfrequenz nur innerhalb derselben CPU-Generation direkt vergleichen!
* **Physische Kerne vs. Logische Threads (SMT / Hyper-Threading):**
  * Oft sind die Rechenwerke (ALU/FPU) eines physischen Kerns nicht zu $100\,\%$ ausgelastet, weil sie z. B. auf Daten aus dem langsamen RAM warten müssen (*Pipeline Bubble / Stall*).
  * Bei **SMT (*Simultaneous Multithreading*)** bekommt ein physischer Kern **zwei getrennte Befehlswarteschlangen und Registersätze**. Für das Betriebssystem sieht 1 physischer Kern wie **2 logische Prozessoren (Threads)** aus (z. B. 8 Kerne / 16 Threads), wodurch Lücken in der Pipeline mit einem zweiten Thread gefüllt werden ($\approx +20\text{–}30\,\%$ Durchsatz bei Multitasking/Rendering).

### 3. CPU-Cache (L1, L2, L3)
Da ein Zugriff auf den normalen Arbeitsspeicher (DRAM) ca. $50\text{–}80\text{ ns}$ dauert (für eine $4\text{ GHz}$-CPU sind das über 200 verschwendete Wartetakte!), sitzt direkt auf dem CPU-Chip ultraschneller **SRAM-Cache**:
* **L1-Cache:** Kleinster ($32\text{–}64\text{ KB}$ pro Kern, aufgeteilt in *L1i* für Befehle und *L1d* für Daten), aber **schnellster Speicher** ($\approx 1\text{ ns}$).
* **L2-Cache:** Größer ($512\text{ KB}\text{ bis }2\text{ MB}$ exklusiv pro Kern), minimal langsamer ($\approx 3\text{–}4\text{ ns}$).
* **L3-Cache (*Last Level Cache*):** Groß ($16\text{ MB}\text{ bis }>128\text{ MB}$), wird **von allen Kernen gemeinsam genutzt** ($\approx 10\text{–}15\text{ ns}$).

### 4. Hardware-Virtualisierung & Stromverbrauch (P-States / C-States)
* **Virtualisierungs-Unterstützung (Intel VT-x / AMD-V):**
  * Spezielle Befehlssatzerweiterung der CPU, die es einem Hypervisor (VMware, Hyper-V, Proxmox, VirtualBox) erlaubt, den Code virtueller Maschinen **direkt und isoliert in Hardware auf der CPU** auszuführen, statt jeden Befehl langsam per Software zu übersetzen.
* **Stromverbrauch bei verschiedenen Leistungsstufen:**
  * Die dynamische Verlustleistung einer CPU folgt dem physikalischen Gesetz:
    $$P_{\text{dyn}} \sim C \cdot U^2 \cdot f \qquad (\text{Kapazität } C \times \text{Spannung } U^2 \times \text{Taktfrequenz } f)$$
  * **P-States (*Performance States*):** Im laufenden Betrieb regelt die CPU je nach Auslastung innerhalb von Millisekunden zwischen niedrigem Basistakt bei geringer Spannung (Office/Idle) und maximalem **Boost-Takt** bei höherer Spannung.
  * **C-States (*Idle / Sleep States*):** Hat ein Kern nichts zu tun, schaltet er sich schrittweise ab (`C0` = aktiv rechnend, `C1` = Takt angehalten, `C6` = Kern komplett von der Stromversorgung getrennt $\rightarrow \approx 0\text{ W}$).

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1 (Spannung & Leistungsaufnahme):** *„Warum steigt der Stromverbrauch und die Hitzeentwicklung einer CPU beim Erhöhen des Taktes um nur 20 % oft um fast 80–100 % an?“*
   * **Lösung:** Damit die Transistoren bei der um $20\,\%$ höheren Taktfrequenz ($f$) noch schnell und fehlerfrei schalten können, muss zwingend auch die **Kernspannung ($U$)** erhöht werden. Da die Spannung in der Leistungsformel $P \sim U^2 \cdot f$ **quadratisch** eingeht, bewirkt schon eine geringe Spannungserhöhung zusammen mit dem höheren Takt einen massiven Anstieg der Verlustleistung.
2. **Frage 2 (SMT vs. echte Kerne):** *„Ist ein 4-Kern-Prozessor mit Hyper-Threading/SMT (8 Threads) genauso schnell wie ein echter 8-Kern-Prozessor ohne SMT bei gleichem Takt? Begründen Sie!“*
   * **Lösung:** Nein, der echte 8-Kerner ist deutlich schneller! Bei SMT teilen sich jeweils zwei Threads **dieselben physischen Rechenwerke (ALUs/FPUs) und denselben L1/L2-Cache** eines einzigen Kerns. SMT nutzt nur die Wartepausen (*Pipeline-Lücken*) dieses einen Kerns besser aus ($\approx +25\,\%$ Mehrleistung), während ein echter 8-Kerner doppelt so viele physische Rechenwerke besitzt.
