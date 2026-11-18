# 🏷️ BfK-I (LF 3) – LS 3: Hosts adressieren und Netzwerkpakete analysieren

> **Fach:** BfK-I (it.schule Stuttgart – 1. Lehrjahr) | **Lernfeld 3:** Clients in Netzwerke einbinden  
> **AP1-Statistik:** *IPv6 (28 P.), MAC-Adresse (10 P.), IPv4 (6 P.), ARP (3 P.), APIPA (2 P.), SLAAC (2 P.)* = **51 Punkte** in der AP1!

---

## ✅ Checkliste der Lehrinhalte (LS 3)
- [ ] **Zahlensysteme** (Dezimal $\leftrightarrow$ Binär $\leftrightarrow$ Hexadezimal) sicher umrechnen
- [ ] **MAC-Adresse (Layer 2):** Aufbau (48 Bit, OUI), Unicast, Multicast, Broadcast (`FF:FF:FF:FF:FF:FF`)
- [ ] **IPv4-Adressierung (Layer 3):** Netz-ID vs. Host-ID per **bitweisem `AND`** bestimmen, Private IP-Bereiche (RFC 1918), Loopback (`127.0.0.1`), APIPA (`169.254.x.x`)
- [ ] **ARP (*Address Resolution Protocol*):** Ablauf von ARP-Request (Broadcast) und ARP-Reply (Unicast)
- [ ] **IPv6-Grundlagen:** 128-Bit-Aufbau, **die 2 Kürzungsregeln**, Adresstypen (`::1`, `fe80::`, `fd00::`, `2000::`, `ff00::`) & **SLAAC**

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Zahlensysteme: 8-Bit-Umrechnungstabelle (1 Oktett = 0 bis 255)

| Bit-Wertigkeit ($2^n$) | $2^7$ | $2^6$ | $2^5$ | $2^4$ | $2^3$ | $2^2$ | $2^1$ | $2^0$ | Dezimalwert | Hexadezimal (2 Nibbles à 4 Bit) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Stellenwert** | **$128$** | **$64$** | **$32$** | **$16$** | **$8$** | **$4$** | **$2$** | **$1$** | Summe | `8-4-2-1` \| `8-4-2-1` |
| *Beispiel 1:* | `1` | `1` | `0` | `0` | `0` | `0` | `0` | `0` | **$192$** | `1100` (`C`) \| `0000` (`0`) = **`0xC0`** |
| *Beispiel 2:* | `1` | `0` | `1` | `0` | `1` | `0` | `0` | `0` | **$168$** | `1010` (`A`) \| `1000` (`8`) = **`0xA8`** |
| *Beispiel 3:* | `1` | `1` | `1` | `1` | `1` | `1` | `1` | `1` | **$255$** | `1111` (`F`) \| `1111` (`F`) = **`0xFF`** |

---

### 2. MAC-Adresse (Layer 2) vs. IP-Adresse (Layer 3)

| Kriterium | MAC-Adresse (*Media Access Control*) | IP-Adresse (*Internet Protocol* v4 / v6) |
| :--- | :--- | :--- |
| **OSI-Schicht** | **Schicht 2 (Sicherungsschicht)** | **Schicht 3 (Vermittlungsschicht)** |
| **Art der Adresse** | **Physische Hardware-Adresse** (fest im Netzwerkchip / NIC eingebrannt, *Burned-In Address*). | **Logische, hierarchische Adresse** (wird vom Admin oder per DHCP/SLAAC passend zum Subnetz vergeben). |
| **Länge & Schreibweise** | **48 Bit (6 Byte)** = 12 Hexadezimalziffern (z. B. `00:1A:2B:3C:4D:5E`). Erste 24 Bit = **OUI (Hersteller-ID)**, letzte 24 Bit = **Geräte-ID**. | **IPv4:** **32 Bit (4 Byte)** dezimal (`192.168.1.10`)<br>**IPv6:** **128 Bit (16 Byte)** hexadezimal (`2001:db8::1`) |
| **Gültigkeitsbereich** | **Nur im lokalen Netzwerksegment (LAN/VLAN)** bis zum nächsten Router! (Wird an jedem Router im Frame-Header ausgetauscht!). | **Ende-zu-Ende durch das gesamte Netzwerk/Internet** vom ursprünglichen Sender bis zum finalen Empfänger! |

---

### 3. IPv4-Adressbereiche & ARP-Ablauf
* **Private IPv4-Adressbereiche nach RFC 1918 (im LAN frei nutzbar, werden im Internet nicht geroutet):**
  * `10.0.0.0/8` (`10.0.0.0` – `10.255.255.255`, Maske `255.0.0.0`)
  * `172.16.0.0/12` (`172.16.0.0` – `172.31.255.255`, Maske `255.240.0.0`)
  * `192.168.0.0/16` (`192.168.0.0` – `192.168.255.255`, Maske `255.255.0.0` bzw. `/24` = `255.255.255.0`)
* **Spezialadressen:**
  * `127.0.0.1` (`127.0.0.0/8`): **Loopback / Localhost** (eigener Host).
  * `169.254.0.0/16` (`169.254.x.x`): **APIPA / Link-Local** – vergibt sich ein Client selbst, wenn er auf DHCP steht, aber **keinen DHCP-Server erreicht**!
* **ARP (*Address Resolution Protocol* – Zuordnung bekannte IPv4 $\rightarrow$ gesuchte MAC):**
  1. PC A prüft durch **bitweises `AND`** mit seiner Subnetzmaske, ob die Ziel-IP im eigenen Subnetz liegt.
  2. Falls ja (und die MAC noch nicht im lokalen **ARP-Cache** `arp -a` steht), sendet PC A einen **ARP-Request als Layer-2-Broadcast (`FF:FF:FF:FF:FF:FF`)** an alle im LAN: *„Wer hat die IP `192.168.1.50`?“*
  3. Nur der Besitzer der IP `192.168.1.50` antwortet mit einem **ARP-Reply per Unicast** direkt an PC A: *„Ich habe diese IP, meine MAC ist `AA:BB:CC:11:22:33`.“*
  *(Liegt die Ziel-IP dagegen in einem **fremden Netz**, fragt PC A per ARP nach der **MAC-Adresse seines Standardgateways/Routers**!)*

---

### 4. IPv6: Kürzungsregeln, Adresstypen & SLAAC (AP1: 30 Punkte!)
* **Aufbau:** **128 Bit** = 8 Blöcke à 16 Bit (4 Hex-Ziffern), z. B. `2001:0db8:0000:0000:0042:0000:0000:0001 /64` (erste 64 Bit = **Netzwerk-Präfix**, letzte 64 Bit = **Interface-Identifier / Host-Anteil**).
* **Die 2 Kürzungsregeln:**
  1. **Führende Nullen** in jedem Block dürfen weggelassen werden (`0db8` $\rightarrow$ `db8`, `0000` $\rightarrow$ `0`). *Endnullen niemals streichen!*
  2. **Ein einziger zusammenhängender Bereich aus Null-Blöcken** darf **einmalig pro Adresse** durch `::` ersetzt werden $\rightarrow$ `2001:db8::42:0:0:1`.
* **Die 5 wichtigsten IPv6-Adresstypen:**
  * **`::1/128`:** **Loopback** (entspricht `127.0.0.1`).
  * **`fe80::/10`:** **Link-Local Unicast** (entspricht APIPA, wird auf jeder IPv6-Schnittstelle automatisch gebildet, wird **nicht** geroutet!).
  * **`fc00::/7` (in der Praxis `fd00::/8`):** **Unique Local Unicast (ULA)** (entspricht privaten IPv4-Adressen nach RFC 1918, intern über mehrere Router routbar).
  * **`2000::/3` (beginnt mit `2...` oder `3...`):** **Global Unicast Address (GUA)** (öffentliche, weltweit im Internet routbare Adresse).
  * **`ff00::/8`:** **Multicast** (ersetzt bei IPv6 auch den Broadcast! `ff02::1` = alle Hosts im LAN, `ff02::2` = alle Router im LAN).
* **SLAAC (*Stateless Address Autoconfiguration*):**
  * Client erzeugt seine Link-Local-Adresse (`fe80::`), prüft per **DAD (*Duplicate Address Detection*)**, ob sie eindeutig ist, fragt per **Router Solicitation (RS an `ff02::2`)** den Router nach dem globalen `/64`-Präfix, erhält dieses per **Router Advertisement (RA)** und kombiniert das Präfix mit seiner eigenen 64-Bit Interface-ID (per EUI-64 oder zufälligen *Privacy Extensions*) zu einer fertigen Global-Unicast-Adresse – **ganz ohne DHCP-Server**!
