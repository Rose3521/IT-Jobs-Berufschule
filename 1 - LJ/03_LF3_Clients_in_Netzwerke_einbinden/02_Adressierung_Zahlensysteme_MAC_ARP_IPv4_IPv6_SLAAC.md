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

---

### 5. IPv6-Vertiefung: RFC 5952, EUI-64, Präfix-Planung, Dual-Stack & Code-Fallen *(Erweitert aus EduSysTrainings)*

#### A. Normgerechte Schreibweise nach RFC 5952 & Spezial-Adressen
Damit jeder dieselbe IPv6-Adresse identisch notiert (wichtig für Prüfungen & Firewall-Regeln!):
1. **Immer die längste Null-Folge** durch `::` ersetzen (bei zwei gleich langen Null-Folgen die **erste** von links).
2. Eine **einzelne** Null-Gruppe (`:0000:`) **NICHT** durch `::` kürzen, sondern als `:0:` schreiben!
3. Alle Hexadezimal-Buchstaben (`a–f`) **kleinschreiben**.
* **Weitere wichtige Spezial-Adressen & Schreibweisen:**
  * **`::`** (*Unspezifizierte Adresse*): Entspricht `0.0.0.0` bei IPv4 (*„noch keine Adresse“* bzw. beim Server-Bind: *„auf allen Schnittstellen lauschen“*).
  * **`2001:db8::/32`**: Offizielles **Dokumentationspräfix** (reserviert für Beispiele, Prüfungsaufgaben und Lehrmaterial).
  * **Zonen-ID unter Windows (`fe80::1c2b:3aff:fe4d:5e6f%12`):** Das `%12` am Ende bezeichnet die Adapter-Nummer, da jeder Netzwerkadapter sein eigenes `fe80::`-Segment besitzt.
  * **⚠️ IPv6 in URLs (Entwicklerfalle!):** Weil Doppelpunkte in einer URL die Portnummer abtrennen, muss eine IPv6-Adresse in einer URL **immer in eckige Klammern `[...]`** gesetzt werden:  
    ✅ `https://[2001:db8:4a00:110::25]:8443/api` *(bei `curl.exe` zusätzlich Option `-g` angeben!)*

#### B. EUI-64 Schritt-für-Schritt von Hand berechnen (aus der 48-Bit MAC-Adresse!)
Will ein Gerät aus seiner MAC-Adresse (z. B. **`A4:5E:60:12:34:56`**) eine 64-Bit-Interface-ID bilden:
1. **MAC-Adresse genau in der Mitte teilen:** `A4:5E:60` | `12:34:56`
2. **Die 2 Bytes `FF:FE` in die Mitte einfügen:** `A4:5E:60:FF:FE:12:34:56` *(daran erkennt man sofort jede EUI-64-Adresse!)*
3. **Im allerersten Byte das 7. Bit von links (*U/L-Bit*) umdrehen (invertieren):**
   * Erstes Byte `A4` in Binär: `1010 0100` $\xrightarrow{\text{7. Bit umdrehen (0 -> 1)}}$ `1010 0110` = **`A6`**!
4. **In 4er-Blöcken kleingeschrieben notieren:** **`a65e:60ff:fe12:3456`**
   * Link-Local: `fe80::a65e:60ff:fe12:3456` | Global Unicast: `2001:db8:77:5:a65e:60ff:fe12:3456`
* **Datenschutzproblem bei EUI-64:** Weil die feste Hardware-MAC direkt in der weltweit sichtbaren IPv6-Adresse steckt, wäre ein Smartphone/Laptop über verschiedene WLANs hinweg eindeutig wiedererkennbar (**Tracking**). Deshalb nutzen moderne Betriebssysteme (Windows, iOS, Android) standardmäßig **zufällige & temporäre Interface-IDs (*Privacy Extensions*)**!

#### C. Hierarchische IPv6-Präfix-Planung an Nibble-Grenzen (4-Bit-Schritte!)
Bei IPv6 rechnet man nicht mit knappen Host-Zahlen wie bei IPv4, sondern teilt das vom Provider erhaltene Präfix sauber an **Hex-Ziffern (Nibbles = 4 Bit)** auf:
* **`/48` (Organisation / Firma / Stadt):** Enthält $2^{8} = \mathbf{256\text{ Standorte à }/56}$ bzw. $2^{16} = \mathbf{65.536\text{ Einzelnetze à }/64}$!
* **`/56` (Ein Standort / Zweigstelle):** Enthält $2^{8} = \mathbf{256\text{ Einzelnetze à }/64}$!
* **`/64` (Ein einzelnes LAN / VLAN / WLAN):** Enthält $2^{64}$ Adressen (**SLAAC erfordert zwingend `/64`!**).

```text
Beispiel-Schema einer Organisation (2001:db8:4a00::/48):
2001:db8:4a00 : 20 | 30 :: /64
└─── /48 ─────┘  │    │
                 │    └── Netz-ID 30 (z. B. Drucker/Technik) -> Bits 57–64
                 └─────── Standort-ID 20 (z. B. Zweigstelle Süd -> /56) -> Bits 49–56
```

#### D. Dual-Stack, Happy Eyeballs & Die 6 häufigsten IPv6-Fallen in Software & Datenbanken
* **Dual-Stack:** IPv4 und IPv6 laufen parallel auf demselben Gerät; DNS liefert sowohl `A`- (IPv4) als auch `AAAA`-Records (IPv6).
* **Priorität & *Happy Eyeballs*:** Betriebssysteme bevorzugen standardmäßig IPv6 (prüfbar in PowerShell mit `Get-NetPrefixPolicy` und `Get-NetIPAddress -AddressFamily IPv6`). Hängt eine IPv6-Verbindung, startet der Browser dank **Happy Eyeballs** nach einem Sekundenbruchteil parallel IPv4 und nimmt die schnellere Verbindung.
* **⚠️ Firewall-Sicherheitsfalle bei Dual-Stack:** Firewall-Regeln gelten **immer pro Protokoll**! Wer nur IPv4-Regeln pflegt, lässt womöglich IPv6 komplett ungeschützt offen!
* **Die 6 häufigsten IPv6-Programmier- & Datenbankfallen:**
  1. **Zu kurzes Datenbankfeld (`VARCHAR(15)`):** Reicht nur für IPv4 (`255.255.255.255` = 15 Zeichen) und schneidet IPv6-Adressen ab! Eine IPv6-Adresse braucht bis zu **39 Zeichen** (mit IPv4-mapped bis **45 Zeichen**) $\rightarrow$ immer **`VARCHAR(45)`** (oder DB-Typ `INET`) nutzen!
  2. **Feste IPs in Config-Dateien:** Besser immer **DNS-Namen** nutzen, damit das OS flexibel über `A`/`AAAA` wählen kann.
  3. **URLs ohne eckige Klammern:** Immer `[IPv6]:Port` schreiben.
  4. **IP-Validierung per Regex:** Statt fehleranfälliger Regex-Muster Standardbibliotheken nutzen (z. B. Python `ipaddress.ip_address()`, C# `IPAddress.TryParse()`).
  5. **Adressvergleich als reiner String:** `2001:DB8::1` und `2001:db8:0:0:0:0:0:1` sind identisch, aber als Text ungleich $\rightarrow$ erst in ein **Adressobjekt** umwandeln, dann vergleichen!
  6. **Server lauscht nur auf `0.0.0.0` (IPv4):** Besser auf **`::` mit Dual-Stack** lauschen (IPv4-Clients erscheinen dann oft als *IPv4-mapped* `::ffff:10.x.x.x`).
