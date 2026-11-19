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

---

### 4. Aufbau eines Ethernet-Frames (Layer 2) & Switch-Verhalten im Detail *(Erweitert aus EduSysTrainings)*

```text
+---------------+----------------+-----------+--------------------+-----------+
| Ziel-MAC      | Quell-MAC      | EtherType | Nutzdaten (Payload)| FCS (CRC) |
| 6 Byte        | 6 Byte         | 2 Byte    | 46 – 1.500 Byte    | 4 Byte    |
+---------------+----------------+-----------+--------------------+-----------+
```

* **EtherType (2 Byte):** Sagt dem Empfänger, welches Protokoll in den Nutzdaten steckt:
  * **`0x0800`** = **IPv4** | **`0x86DD`** = **IPv6** | **`0x0806`** = **ARP** | **`0x8100`** = **IEEE 802.1Q VLAN-Tag**
* **MTU (*Maximum Transmission Unit*):** Die Obergrenze der Nutzdaten in einem Standard-Ethernet-Frame beträgt **$1.500\text{ Byte}$** (größere IP-Pakete müssen fragmentiert werden).
* **FCS (*Frame Check Sequence*, 4 Byte CRC-Prüfsumme):** Der Empfänger rechnet die Prüfsumme nach. Stimmt sie nicht, war der Frame unterwegs beschädigt und **wird auf Schicht 2 stillschweigend verworfen!**
  * *Wichtige Folge:* Schicht 2 sendet verworfene Frames **nicht** neu! Erst **TCP auf Schicht 4** bemerkt nach einem Timeout, dass Daten fehlen, und fordert sie neu an $\rightarrow$ viele beschädigte Frames führen zu extremen **Wartezeiten (Lags)**!
* **Die 3 Grundfunktionen eines Switches:**
  1. **Lernen (*Learning*):** Merkt sich die **Quell-MAC** jedes ankommenden Frames samt Port in der MAC-Adresstabelle (Einträge verfallen nach Inaktivität, oft nach ~5 Minuten).
  2. **Weiterleiten (*Forwarding*):** Steht die **Ziel-MAC** bereits in der Tabelle, geht der Frame gezielt nur an diesen einen Port.
  3. **Fluten (*Flooding*):** Ist die **Ziel-MAC noch unbekannt** (oder ist es ein Broadcast `FF:FF:FF:FF:FF:FF`), schickt der Switch den Frame an **alle Ports außer dem Empfangsport**.
* **Access Point als Layer-2-Brücke:** Ein WLAN-Access-Point leitet Frames drahtloser Geräte (z. B. Handscanner, Laptops) **mit deren eigener MAC-Adresse** ins Kabelnetz weiter – deshalb erscheinen alle verbundenen WLAN-Geräte am selben Uplink-Port in der MAC-Tabelle des Switches!
* **Managed vs. Unmanaged Switch:** Ein *Unmanaged Switch* (20-€-Gerät) lässt sich weder konfigurieren noch auslesen. Ein *Managed Switch* bietet VLANs, Port-Security, **MAC-Tabellen-Einsicht** und **Port-Fehlerzähler** für die Diagnose.

---

### 5. Duplex-Mismatch & Switch-Fehlerzähler auswerten (Praxis-Klassiker!)

* **Autonegotiation:** Beide Seiten einer Leitung handeln beim Einstecken automatisch Geschwindigkeit (`100`/`1000 Mbit/s`) und Duplex (**Vollduplex** = gleichzeitig Senden & Empfangen ohne Kollisionen) aus.
* **Wie entsteht ein Duplex-Mismatch?**
  * Wird **eine Seite fest auf z. B. `100 Mbit/s Vollduplex`** eingestellt, während die **Gegenseite auf `Automatisch` (Autonegotiation)** steht, erkennt die automatische Seite zwar das Tempo (`100 Mbit/s`), bekommt aber keine Duplex-Aushandlung und **fällt gemäß Standard auf Halbduplex zurück!**
  * *Folge:* Die fest eingestellte Seite sendet munter drauflos (Vollduplex), während die Halbduplex-Seite ständig Kollisionen meldet und Frames abbricht. Das Netz „funktioniert noch irgendwie“, wird aber **unter Last extrem langsam**!
* **Port-Fehlerzähler auf einem Managed Switch richtig deuten:**

| Fehlerzähler | Technische Bedeutung | Typische Ursache |
| :--- | :--- | :--- |
| **Kollisionen (*Late Collisions*)** | Zwei Seiten senden gleichzeitig auf einer Leitung (nur bei Halbduplex möglich). | Halbduplex-Betrieb; viele späte Kollisionen = klares Zeichen für **Duplex-Mismatch**! |
| **FCS- / CRC-Fehler** | Prüfsumme am Frame-Ende stimmt nicht $\rightarrow$ Frame wird verworfen. | Defektes/zu langes Kabel, elektromagnetische Störungen oder **Duplex-Mismatch**. |
| **Runts** | Abgebrochene, zu kurze Frames (**$< 64\text{ Byte}$**). | Kollisionen auf der Gegenseite (**Duplex-Mismatch**). |

---

### 6. Schicht-für-Schicht-Diagnose unter Windows & PowerShell

| OSI-Schicht | Leitfrage | Konkreter Windows- / PowerShell-Befehl |
| :---: | :--- | :--- |
| **1 – Bitübertragung** | Kommt ein elektrisches/Funk-Signal an? | Link-LED am Port prüfen, WLAN-Signalstärke messen |
| **2 – Sicherung** | Hat der Adapter eine MAC, Vollduplex & Nachbarn? | `getmac /v` · `arp -a` · `Get-NetAdapter \| Format-Table Name, Status, LinkSpeed, FullDuplex` |
| **3 – Vermittlung** | Hat der Host eine gültige IP & erreicht das Gateway? | `ipconfig /all` · `ping <Gateway-IP>` |
| **4 – Transport** | Lauscht der Dienst auf seinem TCP-Port? | `Test-NetConnection <Host> -Port <Port>` (*prüft `TcpTestSucceeded`*) · `netstat -an \| findstr <Port>` |
| **7 – Anwendung** | Antwortet die Web-App korrekt und schnell? | `curl.exe -i <URL>` (*zeigt HTTP-Statuszeile, z. B. `200 OK`*), Browser-DevTools (`F12` $\rightarrow$ Netzwerk) |
