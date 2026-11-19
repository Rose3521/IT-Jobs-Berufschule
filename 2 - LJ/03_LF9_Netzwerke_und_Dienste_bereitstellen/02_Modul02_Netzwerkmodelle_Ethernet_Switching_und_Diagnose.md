# 🔌 LF 9 – Modul 2: Netzwerkmodelle (OSI & TCP/IP), Ethernet, Switching & Fehlerdiagnose

> **Lernsituation:** *Getränkevertrieb Alsenau KG* (Lagerverwaltungs-App an Hallen-Scannern langsam, im Büro schnell)  
> **EduSysTrainings-Status:** Einheiten `02-01` & `02-02` ✅ eingearbeitet · Einheiten `02-03` & `02-04` 🚧 noch in Vorbereitung

---

## ✅ Checkliste der Lehrinhalte (Modul 2)
- [ ] **OSI-Modell (7 Schichten)** und **TCP/IP-Modell (4 Schichten)** inklusive **Kapselung / PDUs** (*Bits, Frame, Paket, Segment, Daten*) sicher zuordnen
- [ ] Störungen mit **Bottom-up-** und **Top-down-Hypothesen** systematisch Schicht für Schicht per Windows/PowerShell eingrenzen
- [ ] Den Aufbau eines **Ethernet-Frames** (Ziel-MAC, Quell-MAC, EtherType, Nutzdaten/MTU 1500, FCS/CRC) erklären
- [ ] **MAC-Adressen (OUI, Unicast, Broadcast `FF:FF:FF:FF:FF:FF`)** und **ARP** im Zusammenspiel mit IP erklären
- [ ] **Switch-Funktionen** (*Lernen, Weiterleiten, Fluten*), **Access Point als Layer-2-Bridge** sowie **Managed vs. Unmanaged Switches** erläutern
- [ ] **Duplex-Mismatch** sowie Switch-Fehlerzähler (*Kollisionen, FCS-Fehler, Runts*) als typische Performance-Bremse diagnostizieren
- [ ] *(Ausblick 02-03 / 02-04)* Topologien, Medien, Netzkomponenten & Paketmitschnitte (Wireshark) auswerten

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Schichtenmodelle & Kapselung (*Encapsulation*)

| OSI-Schicht | TCP/IP-Schicht | PDU | Kernaufgabe & typische Protokolle |
| :---: | :--- | :--- | :--- |
| **7–5** Anwendung / Darstellung / Sitzung | **Anwendung** | Daten | HTTP(S), DNS, SMTP, SSH, TLS, UTF-8/JSON |
| **4** Transport | **Transport** | Segment / Datagramm | **TCP** (zuverlässig, Ports, Retransmission), **UDP** (verbindungslos) |
| **3** Vermittlung (*Network*) | **Internet** | Paket | **IPv4 / IPv6**, Routing über Netzgrenzen, ICMP |
| **2** Sicherung (*Data Link*) | **Netzzugang** | Frame | **Ethernet (IEEE 802.3)**, WLAN (802.11), **MAC-Adressen**, Switching, FCS/CRC |
| **1** Bitübertragung (*Physical*) | **Netzzugang** | Bits | Kabel (Twisted Pair / LWL), Funk, Stecker, Link-Speed & Duplex |

* **Kapselung beim Senden:** Jede Schicht packt die Daten der oberen Schicht in ihren eigenen Header (und bei Ethernet zusätzlich einen Trailer mit `FCS`) ein – beim Empfänger wird Schicht für Schicht wieder **entkapselt**.

---

### 2. Ethernet-Frame (Layer 2) – Aufbau & Fehlerwirkung

```text
+---------------+---------------+-----------+----------------------+---------+
| Ziel-MAC (6B) | Quell-MAC (6B)| EtherType | Nutzdaten (46–1500 B)| FCS (4B)|
|               |               |   (2 B)   |   (MTU = 1500 Byte)  |  (CRC)  |
+---------------+---------------+-----------+----------------------+---------+
```
* **Ziel- & Quell-MAC (je 48 Bit / 6 Byte):** Erste 3 Byte = **OUI** (*Herstellerkennung*), letzte 3 Byte = Gerätenummer. Broadcast-MAC = `FF:FF:FF:FF:FF:FF`.
* **EtherType (2 Byte):** Bestimmt das gekapselte Protokoll der nächsten Schicht:
  * `0x0800` = **IPv4** · `0x86DD` = **IPv6** · `0x0806` = **ARP** · `0x8100` = **IEEE 802.1Q VLAN-Tag**
* **MTU = 1500 Byte:** Maximale Nutzdatenlänge im Standard-Ethernet-Frame.
* **FCS (*Frame Check Sequence*, 4 Byte CRC):** Prüfsumme gegen Bitfehler. **Wichtige Folge für die Praxis:** Ein Switch verwirft beschädigte Frames **stillschweigend ohne Rückmeldung**! Erst **TCP auf Schicht 4** bemerkt den Verlust per Timeout und sendet erneut $\rightarrow$ **die Verbindung bricht nicht ab, wird aber spürbar zäh/langsam!**

---

### 3. Switching, Access Point & Duplex-Mismatch

#### A. Wie arbeitet ein Switch? (3 Aktionen auf Basis der MAC-Tabelle)
1. **Lernen (*Learning*):** Bei jedem eintreffenden Frame notiert der Switch **`Quell-MAC → Eingangsport`** (mit Aging-Timer, z. B. 300 s).
2. **Weiterleiten (*Forwarding*):** Steht die Ziel-MAC in der Tabelle, geht der Frame gezielt nur an diesen einen Port.
3. **Fluten (*Flooding*):** Ist die Ziel-MAC unbekannt (*Unknown Unicast*) oder `FF:FF:FF:FF:FF:FF` (Broadcast), kopiert der Switch den Frame auf **alle Ports außer dem Eingangsport**.

* **Access Point (AP) = Layer-2-Bridge:** Wandelt WLAN-Frames (802.11) direkt in Ethernet-Frames (802.3) um – er routet nicht und arbeitet in derselben Broadcast-Domäne wie der Switch-Port!
* **Managed vs. Unmanaged Switch:** Nur ein **Managed Switch** liefert **Port-Statistiken / Fehlerzähler**, VLANs (802.1Q), SNMP-Monitoring und Port-Security.

#### B. Duplex-Mismatch & Switch-Fehlerzähler (Klassischer Störungsfall!)
* **Duplex-Mismatch:** Eine Seite steht fest auf **Full-Duplex**, die andere auf **Auto-Negotiation** (fällt ohne Antwort auf **Half-Duplex** zurück!). Folge: Die Full-Duplex-Seite sendet jederzeit, die Half-Duplex-Seite meldet **Spätkollisionen (*Late Collisions*)** und verwirft Pakete $\rightarrow$ extrem langsamer Durchsatz!
  * *Goldene Regel:* **Entweder auf beiden Seiten `Auto-Negotiation` oder auf beiden Seiten fest identisch einstellen!**
* **Fehlerzähler richtig deuten:**
  * **Kollisionen / Late Collisions** $\rightarrow$ Duplex-Mismatch oder veralteter Hub.
  * **FCS- / CRC-Fehler** $\rightarrow$ Defektes Kabel, schlechte Crimpung, elektromagnetische Störung (z. B. Starkstromkabel / Industriemaschine).
  * **Runts** (Frames $< 64\text{ Byte}$) $\rightarrow$ Abgebrochene Übertragungen durch Kollisionen oder defekte Netzwerkkarte.

---

### 4. Systematische Schicht-für-Schicht-Diagnose (Windows / PowerShell)

| OSI | Prüffrage | Windows / PowerShell-Befehl |
| :---: | :--- | :--- |
| **L1** | Link aktiv? Geschwindigkeit & Duplex ok? | `Get-NetAdapter` *(Status `Up`, `LinkSpeed`)* |
| **L2** | Eigene MAC & bekannte Nachbar-MACs im LAN? | `getmac /v` · `arp -a` |
| **L3** | IP, Subnetzmaske, Gateway & Erreichbarkeit (ICMP)? | `ipconfig /all` · `ping <IP>` · `tracert <Ziel>` |
| **L4** | Ist der Ziel-TCP-Port offen / lauscht der Dienst? | `Test-NetConnection <Host> -Port <Port>` · `netstat -an` |
| **L7** | Funktioniert DNS & antwortet HTTP/Web-API korrekt? | `nslookup <Name>` · `curl.exe -i https://<Host>` |

---

### 5. Ausblick: Einheiten `02-03` & `02-04` *(in EduSysTrainings noch 🚧)*
* **Netzkomponenten-Abgrenzung:** **Switch** (L2, trennt Kollisionsdomänen), **Router** (L3, trennt Broadcast-Domänen & verbindet Netze), **Access Point** (L2-Funkbrücke), **Firewall** (L3–L7, filtert nach Regeln & Richtung/Initiator).
* **Paketmitschnitt (Wireshark):** Filter wie `arp`, `icmp`, `tcp.port == 443`, `http`, `dns` zum Nachverfolgen einer Anfrage über alle Schichten.

---

### 🎯 Meilenstein (Modul 2)
* Begründete **Schicht-für-Schicht-Fehlerdiagnose** für die Hallen-Scanner (inkl. Hypothesen, PowerShell-/Switch-Nachweisen und konkreter Behebung).
* *(Querverweis 1. LJ: [LF 3 – Teil 1](../../1%20-%20LJ/03_LF3_Clients_in_Netzwerke_einbinden/01_Netzwerkgrundlagen_OSI_Medien_Verkabelung_Switching.md))*
