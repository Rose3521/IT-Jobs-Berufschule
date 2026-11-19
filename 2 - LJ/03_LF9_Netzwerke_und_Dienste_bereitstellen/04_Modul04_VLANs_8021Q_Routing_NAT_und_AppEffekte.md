# 🧩 LF 9 – Modul 4: Netze segmentieren & verbinden (VLANs 802.1Q, Routing & NAT)

> **Lernsituation:** *Kesselberg Software GmbH* (versehentlicher Testskript-Zugriff auf Produktions-DB $\rightarrow$ Netz sauber in VLANs trennen & Internetzugang absichern)  
> **EduSysTrainings-Status:** Einheiten `04-01` & `04-03` ✅ eingearbeitet · Einheiten `04-02` & `04-04` 🚧 noch in Vorbereitung (Routing- & Firewall-Kernwissen ergänzt)

---

## ✅ Checkliste der Lehrinhalte (Modul 4)
- [ ] **VLAN-Konzept:** Logische Trennung von Broadcast-Domänen auf derselben Hardware (1 Kabel zwischen Etagen!) erklären
- [ ] Den bitgenauen Aufbau des **4-Byte IEEE 802.1Q VLAN-Tags** (`TPID 0x8100`, `PCP`, `DEI`, `VID 1–4094`) beschreiben
- [ ] **Access-Ports (Untagged)**, **Trunk-Ports (Tagged)**, **Native VLAN** und **Management-VLAN** sicher planen (`U`/`T`-Portplan)
- [ ] Begründen, warum **VLANs allein ohne Firewall-Regeln** keine vollständige Sicherheit bieten
- [ ] **Routing-Grundlagen** (Standardgateway, Routingtabelle, Inter-VLAN-Routing) anwenden
- [ ] **Source-NAT / PAT** (Hin- und Rückweg + NAT-Tabelle) sowie **Portweiterleitung (DNAT)** inkl. Sicherheitsregeln beherrschen
- [ ] Die **4 NAT-Effekte auf Anwendungen** (*Idle-Timeout, CGNAT, NAT-Loopback/Hairpinning, Erreichbarkeit hinter NAT*) lösen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. VLANs & IEEE 802.1Q (4-Byte-Tag)
Ein **VLAN** teilt einen physischen Switch logisch in mehrere getrennte Broadcast-Domänen – auch über **ein einziges Kabel (Trunk)** zwischen zwei Switches oder zu einem Multi-SSID-Access-Point.

```text
Standard-Ethernet: [ Ziel-MAC (6B) | Quell-MAC (6B) |              EtherType (2B) | Nutzdaten | FCS ]
Mit 802.1Q-Tag:    [ Ziel-MAC (6B) | Quell-MAC (6B) | 802.1Q (4B) | EtherType (2B) | Nutzdaten | FCS ]
                                                      └── TPID 0x8100 (16b) | PCP (3b) | DEI (1b) | VID (12b)
```
* **`TPID = 0x8100` (16 Bit):** Kennzeichnet, dass ein 802.1Q-VLAN-Tag folgt.
* **`PCP` (3 Bit):** Priorität (QoS, z. B. für VoIP).
* **`DEI` (1 Bit):** *Drop Eligible Indicator* (Verwerf-Markierung bei Überlast).
* **`VID` (12 Bit):** VLAN-ID (`1` bis `4094`; `0` und `4095` sind reserviert).

#### Wichtige VLAN-Planungsregeln (5-Schritte-Rezept)
1. **Access-Port (`U` = Untagged):** Für Endgeräte (PC, Drucker, Server) – genau 1 VLAN; der Switch entfernt das Tag beim Senden ans Endgerät.
2. **Trunk-Port (`T` = Tagged):** Zwischen Switches, zum Router/Firewall oder zum Multi-SSID-Access-Point (mehrere VLANs über 1 Kabel).
3. **VLAN 1 meiden & eigenes Management-VLAN nutzen:** Standard-VLAN 1 trägt oft ungetaggten Default-Traffic; Switch-/AP-Verwaltung gehört in ein separates **Management-VLAN** (z. B. VLAN 99).
4. **Native VLAN:** Ungetaggte Frames auf einem Trunk landen im Native VLAN – auf beiden Seiten identisch und niemals auf ein Produktivnetz legen!
5. **⚠️ Wichtig:** **VLANs allein ersetzen keine Firewall!** Sobald ein Router zwischen VLANs routet, braucht es explizite **Firewall-Regeln** (*Default Deny*, nur nötige Verbindungen wie `Prod-App → Prod-DB:5432` erlauben, `Dev → Prod-DB` blockieren!).

* **5-Schritte-Planung:** (1) Zonen/Rollen festlegen *(Prod, Test, Dev, Büro, Gäste, Mgmt)* $\rightarrow$ (2) VLAN-IDs & Subnetze vergeben (z. B. `VLAN 30 → 10.20.30.0/24`) $\rightarrow$ (3) **VLAN-Tabelle** schreiben $\rightarrow$ (4) **Portplan (`U`/`T`)** je Switch & AP erstellen $\rightarrow$ (5) Firewall-/Kommunikationsregeln definieren.

---

### 2. Routing-Grundlagen & Inter-VLAN-Routing *(Ausblick `04-02`)*
* **Standardgateway:** Ziel-IP liegt außerhalb des eigenen Subnetzes $\rightarrow$ Host sendet Frame an die MAC-Adresse seines Standardgateways (Router).
* **Routingtabelle:** Router wählt immer den spezifischsten passenden Eintrag (**Longest Prefix Match**, z. B. `/24` vor Default-Route `0.0.0.0/0`).
* **Inter-VLAN-Routing:** Entweder per **Router-on-a-Stick** (1 Trunk-Kabel zum Router/Firewall mit Subinterfaces je VLAN) oder per **Layer-3-Switch** (SVIs).

---

### 3. NAT / PAT, Portweiterleitung (DNAT) & 4 NAT-Effekte auf Apps

#### A. Source-NAT / PAT (Viele private IPs $\rightarrow$ 1 öffentliche IPv4)
* **Hinweg:** Router ersetzt private Quell-IP + Quell-Port (z. B. `10.20.10.15:52100`) durch seine öffentliche IP + freien Port (z. B. `203.0.113.42:41001`) und merkt sich die Zuordnung in der **NAT-Tabelle**.
* **Rückweg:** Antwort an `203.0.113.42:41001` wird anhand der NAT-Tabelle zurück auf `10.20.10.15:52100` übersetzt.

#### B. Portweiterleitung (Destination-NAT / DNAT) & Sicherheitsregeln
* Leitet eingehende Anfragen von der öffentlichen IP + Port (z. B. `203.0.113.42:443`) an einen internen Server (z. B. `10.20.40.10:443`) weiter.
* **Strikte Sicherheitsregeln:**
  * **Nur zwingend öffentliche Dienste** weiterleiten (z. B. `HTTPS 443` auf Reverse Proxy / Web-App in der DMZ).
  * **NIEMALS direkt ins Internet weiterleiten:** Datenbanken (`5432` PostgreSQL, `3306` MySQL), Admin-Zugänge (`SSH 22`, `RDP 3389`) oder vergessene Demo-/Test-Server $\rightarrow$ dafür **VPN** nutzen!
  * **Anderer Port hilft nicht (*Security by Obscurity*):** Port-Scanner finden Dienste auf `8443` oder `2222` in wenigen Sekunden.

#### C. Die 4 typischen NAT-Effekte auf Anwendungen (wichtig für Entwickler!)
| NAT-Effekt | Problem in der Praxis | Saubere Lösung |
| :--- | :--- | :--- |
| **1. Idle-Timeout** *(z. B. 300 s)* | Langlebige TCP-Verbindungen (Live-Dashboard) werden bei Ruhe stillschweigend aus der NAT-Tabelle gelöscht. | **Heartbeat / TCP-Keep-Alive** in kürzerem Intervall (z. B. alle 60 s) senden. |
| **2. CGNAT / geteilte IP** | Hunderte Mobilfunk- oder Firmennutzer teilen sich dieselbe öffentliche IP $\rightarrow$ IP-basiertes Rate-Limiting sperrt Unschuldige. | **Account-/Token-basiertes Rate-Limiting** statt reiner IP-Sperre. |
| **3. NAT-Loopback (*Hairpinning*)** | Interner Client ruft `https://app.firma.de` (öffentliche Router-IP) auf – Router unterstützt kein Hairpinning $\rightarrow$ Fehler! | **Split-Horizon-DNS:** Interner DNS löst den Namen direkt auf die interne Server-IP auf. |
| **4. Client hinter NAT nicht direkt erreichbar** | Server kann keine TCP-Verbindung aktiv zum Smartphone/Client hinter einem NAT-Router initiieren. | **Client baut Verbindung ausgehend auf** (WebSocket, Long Polling) oder Nutzung von **Push-Diensten**. |

---

### 🎯 Meilenstein (Modul 4)
* Vollständiges **Segmentierungs- & Internetkonzept** (VLAN-Tabelle, Switch-/AP-Portplan `U`/`T`, NAT-/Portweiterleitungs-Audit und Firewall-Regeln).
* *(Querverweis 1. LJ: [LF 3 – Teil 5 (VLAN)](../../1%20-%20LJ/03_LF3_Clients_in_Netzwerke_einbinden/05_VLANs_8021Q_und_WLAN_Funknetze_WPA2_WPA3.md) & [LF 3 – Teil 6 (NAT)](../../1%20-%20LJ/03_LF3_Clients_in_Netzwerke_einbinden/06_Netzwerkdienste_TCP_UDP_Ports_DHCP_DNS_NAT_Diagnose.md))*
