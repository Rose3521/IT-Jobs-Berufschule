# 🔋 LF 2 – Geräteklassen (Thin/Fat/Zero Client), Netzwerkspeicher (NAS/SAN) & USV (LS 9)

> **Lernfeld 2 & BfK-I LS 9:** Arbeitsplätze ausstatten / Serverraum & Rechenzentrum  
> **AP1-Statistik:** *Geräteklassen, NAS/SAN & Stromversorgung* kamen regelmäßig in der AP1 vor!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Geräteklassen:** Fat Client vs. Thin Client vs. Zero Client vergleichen
- [ ] **Speicherarchitekturen:** DAS vs. NAS vs. SAN unterscheiden
- [ ] **USV-Klassifizierung (DIN EN 62040-3):** VFD (Offline), VI (Line-Interactive), VFI (Online) zuordnen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Geräteklassen: Fat Client vs. Thin Client vs. Zero Client (AP1 2023F)

| Geräteklasse | Funktionsweise & Merkmale | Vorteile | Nachteile |
| :--- | :--- | :--- | :--- |
| **Fat Client (Rich Client)** | Vollwertiger Desktop-PC oder Laptop mit starker CPU, viel RAM, lokalem Betriebssystem und lokal installierten Programmen. | Eigene hohe Rechenleistung (für CAD, Video, Entwicklung), **arbeitet auch bei Netzwerk-/Serverausfall lokal weiter**. | Hoher Stromverbrauch, hoher administrativer Aufwand an jedem Einzelplatz, höheres Risiko bei Diebstahl lokaler Daten. |
| **Thin Client** | Kleiner, lüfterloser Mini-PC mit minimalem OS (z. B. Embedded Linux / Windows IoT). Alle Anwendungen laufen zentral auf einem **Terminalserver / VDI im Rechenzentrum** (Zugriff per RDP / Citrix). | **Sehr geringer Stromverbrauch (Green IT!)**, zentrale Administration & Updates auf dem Server, hohe Datensicherheit (keine lokalen Nutzdaten), lange Lebensdauer. | **Totale Abhängigkeit von Netzwerk & Terminalserver** (bei Server-/Netzausfall steht die Arbeit still!); kaum für 3D/CAD geeignet. |
| **Zero Client** | Besitzt **gar kein eigenes Betriebssystem** und keinen allgemeinen Flash-Speicher mehr, sondern nur noch einen hochspezialisierten Dekodier-Chip (ASIC) für das VDI-Protokoll. | Startet in wenigen Sekunden, komplett wartungsfrei, immun gegen lokale Malware. |An ein einziges Virtualisierungsprotokoll gebunden. |

---

### 2. Speicherarchitekturen: DAS vs. NAS vs. SAN (AP1 2022H)
* **DAS (*Direct Attached Storage*):** Direkt an einen einzelnen Rechner angeschlossener Speicher (interne SSD/HDD, externe USB-Festplatte). Kein direkter Zugriff für andere Clients im Netz.
* **NAS (*Network Attached Storage*):** Eigener kleiner Dateiserver im lokalen Netzwerk (Ethernet / TCP/IP). Stellt Speicherplatz auf **Dateiebene (*File-Level*)** über Protokolle wie **SMB/CIFS** (Windows) oder **NFS** (Linux) bereit. Ideal für gemeinsame Abteilungsordner.
* **SAN (*Storage Area Network*):** Hochgeschwindigkeits-Speichernetzwerk im Rechenzentrum (über **Fibre Channel** oder **iSCSI**), das Servern Speicher auf **Blockebene (*Block-Level*)** bereitstellt (der Server bindet das SAN-Volume wie eine lokale physische Festplatte ein und formatiert es mit seinem eigenen Dateisystem). Ideal für Datenbankserver und VMware-/Hyper-V-Cluster.

---

### 3. Unterbrechungsfreie Stromversorgung – USV-Klassen nach IEC 62040-3 (LS 9 & AP1)

| USV-Klasse | Bezeichnung | Funktionsweise & Umschaltzeit | Schützt gegen... | Typischer Einsatz |
| :--- | :--- | :--- | :--- | :--- |
| **VFD** (*Voltage and Frequency Dependent*) | **Offline / Standby-USV** | Leitet im Normalbetrieb den Netzstrom direkt durch; schaltet erst bei Ausfall auf Batterie/Wechselrichter um (**Umschaltzeit: ca. 4–10 ms**). | **Nur** kompletter Stromausfall & kurze Spannungsspitzen | Einzelplatz-PCs, Peripherie, unkritische Geräte |
| **VI** (*Voltage Independent*) | **Line-Interactive-USV** | Besitzt einen automatischen Spannungsregler (**AVR**), der Unter- und Überspannungen ohne Batterieentladung ausgleicht (**Umschaltzeit bei Ausfall: ca. 2–4 ms**). | Stromausfall **+** dauerhafte Spannungsschwankungen (*Brownouts* / Überspannung) | Switches, Router, NAS, Abteilungsserver |
| **VFI** (*Voltage and Frequency Independent*) | **Online / Dauerwandler-USV** | Strom fließt **immer** über Gleichrichter $\rightarrow$ Batterie/Zwischenkreis $\rightarrow$ Wechselrichter (**Umschaltzeit: 0 ms!**). Erzeugt permanent eine völlig neue, saubere Sinusspannung. | **Alle** Netzstörungen (Ausfall, Spannungsschwankungen, **Frequenzschwankungen**, Oberschwingungen, Blitzeinschläge) | Rechenzentren, kritische Produktivserver, Medizintechnik |
