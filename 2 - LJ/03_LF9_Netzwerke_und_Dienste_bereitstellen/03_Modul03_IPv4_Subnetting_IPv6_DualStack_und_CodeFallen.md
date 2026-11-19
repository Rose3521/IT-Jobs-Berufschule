# 🔢 LF 9 – Modul 3: IP-Adressierung mit IPv4 & IPv6, Präfixplanung, Dual-Stack & Code-Fallen

> **Lernsituation:** *Stadtbücherei Wiesenfeld* (Zweigstelle mit Selbstverbuchung, Katalog-PCs & Gäste-WLAN inkl. IPv6-Konzept)  
> **EduSysTrainings-Status:** Einheiten `03-04` & `03-05` ✅ eingearbeitet · Einheiten `03-01` bis `03-03` 🚧 noch in Vorbereitung (IPv4-Subnetting-Basis aus 1. LJ verknüpft)

---

## ✅ Checkliste der Lehrinhalte (Modul 3)
- [ ] **IPv4-Subnetting (CIDR/VLSM):** Netz-ID, Broadcast, Host-Bereich und Host-Anzahl ($2^h - 2$) fehlerfrei berechnen *(siehe auch 1. LJ LF 3 – Teil 4)*
- [ ] **IPv6-Kürzungsregeln nach RFC 5952** fehlerfrei anwenden und gekürzte Adressen vollständig ausschreiben
- [ ] **IPv6-Adresstypen** (GUA, ULA, Link-Local `fe80::/10`, Loopback `::1`, `::`, Dokumentation `2001:db8::/32`) sowie **Zone-ID (`%12`)** und **URL-Notation `[IPv6]:Port`** anwenden
- [ ] **EUI-64** Schritt für Schritt aus einer MAC-Adresse berechnen und das Datenschutzproblem (*Tracking* vs. Privacy Extensions) erklären
- [ ] **Hierarchische IPv6-Präfixplanung** an Nibble-Grenzen (`/48` $\rightarrow$ `/56` $\rightarrow$ `/64`) erstellen
- [ ] **Dual-Stack**, **Happy Eyeballs** (`Get-NetPrefixPolicy`) und die **Dual-Stack-Firewall-Falle** erklären
- [ ] Die **6 typischen IPv6-Code- und Datenbank-Fallen** in Softwareprojekten erkennen und vermeiden

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. IPv4-Subnetting (Kompakt-Recap für `03-01` bis `03-03`)
* **Aufbau:** 32 Bit (4 Oktette), getrennt in **Netzanteil** (`/n`) und **Hostanteil** ($h = 32 - n$).
* **Nutzbare Hosts pro Subnetz:** $2^h - 2$ (abzüglich Netz-ID und Broadcast-Adresse).
* **Private Bereiche (RFC 1918):** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.
* *(Ausführliche CIDR-Tabelle & Rechenbeispiele: [1. LJ – LF 3 Teil 4](../../1%20-%20LJ/03_LF3_Clients_in_Netzwerke_einbinden/04_Subnetting_IPv4_CIDR_Berechnung_und_Beispiele.md))*

---

### 2. IPv6: Schreibweise (RFC 5952), Sonderadressen & URL-Notation
Eine IPv6-Adresse ist **128 Bit** lang: **8 Gruppen à 4 Hex-Ziffern (16 Bit)**, getrennt durch Doppelpunkte (`:`).

#### Kanonische Kürzungsregeln nach RFC 5952
1. **Kleinbuchstaben** verwenden (`a–f`, nicht `A–F`).
2. **Führende Nullen** innerhalb jeder 4er-Gruppe streichen (`0042` $\rightarrow$ `42`, `0000` $\rightarrow$ `0`).
3. **`::` höchstens EINMAL** für die **längste Folge von mindestens zwei** reinen `0`-Gruppen verwenden *(eine einzelne `:0:`-Gruppe bleibt `:0:`!; bei Gleichstand gewinnt die **linke** Folge)*.

#### Wichtige Sonderadressen & Schreibweisen
| Adresse / Präfix | Bedeutung |
| :--- | :--- |
| **`2000::/3`** | **Global Unicast Address (GUA)** – weltweit im Internet routbar |
| **`fc00::/7`** *(Praxis: `fd00::/8`)* | **Unique Local Address (ULA)** – nur intern, nicht im Internet routbar |
| **`fe80::/10`** | **Link-Local** – auf jedem Interface aktiv, nur im lokalen Segment (nicht geroutet!) |
| **`::1/128`** | Loopback (`localhost`) |
| **`::/128`** | Unspezifizierte Adresse („alle Interfaces“ beim Server-Bind / noch keine Adresse) |
| **`2001:db8::/32`** | **Dokumentationspräfix** (RFC 3849 – ausschließlich für Lehrmaterial/Beispiele!) |
| **`fe80::1%12`** | **Zone-ID (`%12`):** Legt bei Link-Local-Adressen die konkrete Netzwerkkarte fest |
| **`http://[2001:db8::1]:8080/`** | **IPv6 in URLs:** Immer in eckige Klammern `[...]`, damit der Port-Doppelpunkt eindeutig bleibt (`curl.exe -g` nötig, da sonst Globbing-Fehler!) |

---

### 3. SLAAC, EUI-64 & Datenschutz (Privacy Extensions)
Bei **SLAAC** schickt der Router ein `/64`-Präfix (*Router Advertisement*) und der Client bildet seine 64-Bit-Interface-ID selbst:
* **EUI-64 aus MAC-Adresse berechnen (4 Schritte):**
  1. 48-Bit-MAC in der Mitte teilen (`AA:BB:CC` | `DD:EE:FF`)
  2. **`FF:FE`** in die Mitte einfügen $\rightarrow$ `AA:BB:CC:FF:FE:DD:EE:FF`
  3. **7. Bit (U/L-Bit) im ersten Byte invertieren** (XOR mit `0x02`, z. B. `0x00` $\rightarrow$ `0x02`, `0xA2` $\rightarrow$ `0xA0`)
  4. In 16-Bit-Gruppen schreiben $\rightarrow$ `a8bb:ccff:fedd:eeff`
* **Datenschutzproblem:** EUI-64 verrät weltweit die feste Hardware-MAC (Gerät über verschiedene Netze trackbar!). Moderne Betriebssysteme nutzen daher **Privacy Extensions (RFC 8981)** mit wechselnden zufälligen Interface-IDs für ausgehende Verbindungen.

---

### 4. Hierarchische IPv6-Präfixplanung an Nibble-Grenzen
Ein Hex-Zeichen = **1 Nibble = 4 Bit**. Präfixe teilt man sauber an 4-Bit-Grenzen:
* **`/48`** (Unternehmen / Organisation) $\rightarrow$ 16 Bit Subnet-ID = $2^{16} = \mathbf{65.536}$ `/64`-Subnetze
* **`/56`** (Standort / Zweigstelle / Geschäftsanschluss) $\rightarrow$ 8 Bit Subnet-ID = $2^8 = \mathbf{256}$ `/64`-Subnetze
* **`/64`** (**Standardgröße für JEDES LAN/VLAN!** – zwingend für SLAAC!)
* *Beispiel (Nibble-Schema):* Standort-Präfix `2001:db8:4200::/56` $\rightarrow$ VLAN 10 (Verwaltung) = `2001:db8:4210::/64`, VLAN 20 (Katalog) = `2001:db8:4220::/64`, VLAN 30 (Gäste) = `2001:db8:4230::/64`.

---

### 5. Dual-Stack, Happy Eyeballs & 6 IPv6-Code-/Datenbank-Fallen
* **Dual-Stack:** Host und Netz sprechen gleichzeitig IPv4 und IPv6.
* **Happy Eyeballs (RFC 8305):** Browser/Apps testen IPv6 zuerst und starten bei ausbleibender Antwort nach wenigen Millisekunden parallel IPv4 – der Nutzer merkt defektes IPv6 oft nicht. Windows-Präferenz prüfen: `Get-NetPrefixPolicy` & `Get-NetIPAddress`.
* **⚠️ Dual-Stack-Firewall-Falle:** Wer nur IPv4-Firewallregeln pflegt, lässt das **gesamte IPv6-Netz ungeschützt offen**! Sicherheitsregeln müssen immer **für beide Protokolle** gelten!

#### Die 6 typischen IPv6-Fallen für Entwickler & Admins
1. **Datenbankspalte zu kurz:** `VARCHAR(15)` reicht nur für IPv4 (`255.255.255.255`) – IPv6 braucht bis zu 39 Zeichen (mit IPv4-mapped `::ffff:255.255.255.255` **45 Zeichen**) $\rightarrow$ **`VARCHAR(45)`** (oder nativer Typ wie `INET`)!
2. **Hardcodierte IPs statt DNS-Namen:** Immer Hostnamen konfigurieren, damit der Client automatisch `AAAA`/`A` auflösen kann.
3. **URL-Parsing ohne Klammern:** `host + ":" + port` zerbricht bei IPv6 $\rightarrow$ Standard-URI-Builder bzw. `[IPv6]:Port` verwenden!
4. **Eigene Regex für IP-Validierung:** Scheitert an `::`-Varianten $\rightarrow$ immer Standardbibliotheken nutzen (C#: `IPAddress.TryParse`, Python: `ipaddress.ip_address`).
5. **String-Vergleich statt Adressvergleich:** `"2001:0db8::1" != "2001:db8::1"` als Text, obwohl es dieselbe Adresse ist $\rightarrow$ immer geparste **IP-Objekte** vergleichen!
6. **Server-Bind & IPv4-mapped:** Lauschen auf `0.0.0.0` nimmt nur IPv4 an; für Dual-Stack auf `::` (*IPv6Any*) binden (IPv4-Clients erscheinen in Logs teils als `::ffff:192.0.2.10`).

---

### 🎯 Meilenstein (Modul 3)
* Vollständiges **Dual-Stack-Adresskonzept (IPv4 + IPv6)** für die Zweigstelle inkl. IPv6-Code-/Datenbank-Prüfbericht.
* *(Querverweis 1. LJ: [LF 3 – Teil 2](../../1%20-%20LJ/03_LF3_Clients_in_Netzwerke_einbinden/02_Adressierung_Zahlensysteme_MAC_ARP_IPv4_IPv6_SLAAC.md))*
