# 🔌 BfK-I (LF 3) – LS 2: Hosts und Netzwerkkomponenten

> **Fach:** BfK-I (it.schule Stuttgart – 1. Lehrjahr) | **Lernfeld 3:** Clients in Netzwerke einbinden  
> **AP1-Statistik:** *Cluster Netzwerktechnik: 91 % Häufigkeit / 140 Punkte* (*ISO/OSI*: 18 P., *Netzwerkkabel & RJ45*: 12 P.)

---

## ✅ Checkliste der Lehrinhalte (LS 2)
- [ ] **ISO/OSI-7-Schichtenmodell** vs. **TCP/IP-4-Schichtenmodell** (Schichten, PDUs, Protokolle, Kopplungselemente)
- [ ] **Datenkapselung (*Encapsulation*)** beim Versand durch die Schichten erklären
- [ ] **Übertragungsmodi:** Simplex, Halb-Duplex (CSMA/CD, CSMA/CA) und Voll-Duplex unterscheiden
- [ ] **Kollisionsdomäne vs. Broadcastdomäne** an Hub, Switch und Router abgrenzen
- [ ] **Strukturierte Verkabelung (EN 50173)** sowie **Kupferkabel (Twisted Pair Cat 5e–8)** vs. **Glasfaser (LWL Singlemode/Multimode)** & **PoE** vergleichen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Das ISO/OSI-7-Schichtenmodell & TCP/IP-Modell

| OSI-Schicht | Name (DE / EN) | TCP/IP-Schicht | PDU (Dateneinheit) | Protokolle / Standards | Kopplungselemente | Hauptaufgabe |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **7** | **Anwendung** (*Application*) | Anwendung | Daten | HTTP(S), FTP, SMTP, IMAP, DNS, DHCP, SSH, SMB | Application-Firewall, Proxy, Gateway | Schnittstelle zur Anwendungssoftware |
| **6** | **Darstellung** (*Presentation*) | Anwendung | Daten | TLS/SSL, UTF-8, ASCII, JSON, JPEG | Gateway | Datenformatierung, Verschlüsselung, Kompression |
| **5** | **Sitzung** (*Session*) | Anwendung | Daten | NetBIOS, RPC, SIP | Gateway | Sitzungsauf-, -abbau und Synchronisation |
| **4** | **Transport** (*Transport*) | Transport | **Segment** (TCP) / **Datagramm** (UDP) | **TCP, UDP** | Layer-4-Switch, Stateful Firewall | Ende-zu-Ende-Kontrolle, **Port-Adressierung** |
| **3** | **Vermittlung** (*Network*) | Internet | **Paket** (*Packet*) | **IPv4, IPv6, ICMP, IPSec**, OSPF | **Router**, Layer-3-Switch | Logische Adressierung (**IP-Adresse**) & **Routing** |
| **2** | **Sicherung** (*Data Link*) | Netzzugang | **Rahmen** (*Frame*) | **Ethernet (802.3), WLAN (802.11), VLAN (802.1Q), ARP** | **Switch**, Bridge, Access Point | Physische Adressierung (**MAC-Adresse**), FCS-Prüfsumme |
| **1** | **Bitübertragung** (*Physical*) | Netzzugang | **Bit** | RJ45, 1000BASE-T, LWL, Funkwellen | **Hub, Repeater**, Medienkonverter, Kabel | Elektrische, optische oder elektromagnetische Signalübertragung |

---

### 2. Kollisionsdomäne vs. Broadcastdomäne & Arbeitsweise eines Switches
* **Hub / Repeater (Layer 1):** Verstärkt eingehende Bits blind an alle anderen Ports weiter $\rightarrow$ **1 große gemeinsame Kollisionsdomäne** und **1 Broadcastdomäne** (Halb-Duplex mit **CSMA/CD** nötig).
* **Switch (Layer 2):**
  * Lernt anhand der **Quell-MAC-Adresse** eingehender Frames selbstständig, an welchem Port welcher PC hängt, und trägt dies in seine **MAC-Adresstabelle (*Source-Address-Learning*)** ein.
  * Kennt er die Ziel-MAC bereits, schickt er den Frame **gezielt nur an diesen einen Ziel-Port** (im **Voll-Duplex** ohne Kollisionen!).
  * **Wichtig:** An einem Switch bildet **jeder einzelne Port eine eigene Kollisionsdomäne** (ein 24-Port-Switch mit 24 PCs hat **24 Kollisionsdomänen**, aber **nur 1 gemeinsame Broadcastdomäne**, solange keine VLANs eingerichtet sind!).
* **Router (Layer 3):**
  * Leitet Broadcasts (`FF:FF:FF:FF:FF:FF` / `255.255.255.255`) grundsätzlich **NICHT** weiter $\rightarrow$ **Jedes Router-Interface (und jedes VLAN) begrenzt eine eigene Broadcastdomäne!**

---

### 3. Strukturierte Verkabelung (EN 50173) & Übertragungsmedien

| Verkabelungsbereich | Verbindet... | Eingesetztes Medium | Max. Länge / Grund |
| :--- | :--- | :--- | :--- |
| **Primärbereich** *(Gelände / Campus)* | Verschiedene **Gebäude** (Standortverteiler $\leftrightarrow$ Gebäudeverteiler) | **LWL (Glasfaser – meist Singlemode)** | Bis mehrere Kilometer; hohe Bandbreite, **galvanische Trennung** (kein Blitzschlag-/Potenzialproblem zwischen Gebäuden!). |
| **Sekundärbereich** *(Steigleitung / Gebäude)* | Verschiedene **Etagen** im Gebäude (Gebäudeverteiler $\leftrightarrow$ Etagenverteiler) | **LWL (Multimode)** oder Cat-7-Kupfer | Hohe Backbone-Bandbreite für alle Stockwerke. |
| **Tertiärbereich** *(Etage / Horizontal)* | **Etagenverteiler (Patchpanel/Switch) $\leftrightarrow$ Anschlussdose** im Büro | **Kupfer-Twisted-Pair** (Cat 7 Verlegekabel + Cat 6a RJ45-Dose) | **Max. 100 Meter Gesamtlänge** ($90\text{ m}$ festes Verlegekabel + max. $2 \times 5\text{ m}$ Patchkabel). Unterstützt **PoE (Power over Ethernet)**! |

#### Vergleich: Kupfer (Twisted Pair) vs. Lichtwellenleiter (LWL / Glasfaser)
* **Kupfer (Twisted Pair – Cat 5e bis Cat 8):**
  * *Schirmungsarten:* **U/UTP** (ungeschirmt), **F/UTP** (Gesamt-Folienschirm), **S/FTP** (Gesamt-Geflechtschirm + Folie um jedes einzelne Adernpaar – bester Schutz gegen Nebensprechen/*Crosstalk*!).
  * *Vorteil:* Günstig, robust beim Biegen, einfach zu patchen, überträgt **Strom per PoE (IEEE 802.3af/at/bt)** für Access Points, IP-Telefone und Kameras.
  * *Nachteil:* Starke Dämpfung ab $100\text{ m}$, empfindlich gegen elektromagnetische Störfelder (EMV), kein Potenzialausgleich.
* **Lichtwellenleiter (LWL – Glasfaser):**
  * **Multimode-Faser (MMF):** Dickerer Faserkern ($50\text{ µm}$), Licht breitet sich auf mehreren Wegen (*Moden*) gleichzeitig aus $\rightarrow$ kostengünstige LED/VCSEL-Laser, für mittlere Strecken bis ca. $500\text{ m}$ (Gebäudeverkabelung).
  * **Singlemode-/Monomode-Faser (SMF):** Extrem dünner Faserkern ($9\text{ µm}$), Licht läuft auf exakt einem geraden Strahl ohne Modendispersion $\rightarrow$ für weite Strecken von **vielen Kilometern** (Campus, Stadtnetze, Tiefseekabel).
  * *Vorteile LWL:* Extrem hohe Bandbreite (Terabit/s), **100 % unempfindlich gegen elektromagnetische Störungen**, abhörsicher, galvanische Trennung.
