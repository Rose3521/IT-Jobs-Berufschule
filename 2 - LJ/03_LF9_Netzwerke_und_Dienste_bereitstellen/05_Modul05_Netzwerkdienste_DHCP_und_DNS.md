# 🏷️ LF 9 – Modul 5: Grundlegende Netzwerkdienste (DHCP & DNS)

> **Lernsituation:** *Hotel Seeblick Arnsee* (interne Web-App unter festem Namen erreichbar machen; neue Tablets automatisch ins Netz aufnehmen)  
> **EduSysTrainings-Status:** 🟡 Einheiten `05-01` bis `05-04` im Lehrerprojekt noch 🚧 *In Vorbereitung* – hier das prüfungsrelevante Kernwissen als Arbeitsgrundlage.

---

## ✅ Checkliste der Lehrinhalte (Modul 5)
- [ ] **DHCP (DORA-Ablauf):** `Discover` $\rightarrow$ `Offer` $\rightarrow$ `Request` $\rightarrow$ `Acknowledge` inkl. Lease-Zeit, Pool, Ausschlüssen und **MAC-Reservierungen** erklären
- [ ] **Wichtige DHCP-Optionen** (Subnetzmaske, Standardgateway, DNS-Server, Domain-Name) benennen
- [ ] **DNS-Namensauflösung:** Hierarchie (Root `.`, TLD `.de`, Domain, Host), rekursive vs. iterative Auflösung und DNS-Caching erklären
- [ ] **Wichtige DNS-Resource-Records** (`A`, `AAAA`, `CNAME`, `PTR`, `MX`, `TXT`, `SRV`, `NS`, `SOA`) für eigene Anwendungen planen
- [ ] **Split-Horizon-DNS** (intern vs. extern) sowie **Reverse-DNS (`PTR`)** begründen
- [ ] DHCP und DNS in Windows/Linux überprüfen (`ipconfig /all`, `ipconfig /renew`, `nslookup`, `Resolve-DnsName`, `dig`)

---

## 📌 Kernwissen & Prüfungsgrundlagen

### 1. DHCP (*Dynamic Host Configuration Protocol* – UDP 67 Server / UDP 68 Client)
Vergibt IP-Konfiguration automatisch an Clients, damit neue Endgeräte (z. B. Tablets) ohne Handarbeit ins Netz gelangen:
1. **D – Discover** *(Client $\rightarrow$ Broadcast)*: „Gibt es hier einen DHCP-Server?“
2. **O – Offer** *(Server $\rightarrow$ Client)*: „Ich biete dir IP `10.20.10.50` inkl. Maske, Gateway, DNS & Lease-Zeit an.“
3. **R – Request** *(Client $\rightarrow$ Broadcast)*: „Ich nehme dieses Angebot von Server X an.“
4. **A – Acknowledge** *(Server $\rightarrow$ Client)*: „Bestätigt – die Adresse ist für dich reserviert.“

* **Praxisregeln für DHCP-Planung:**
  * **Statische IPs / Ausschlussbereich** für Infrastruktur & Server (Router, Switches, APs, Web-/DB-Server), z. B. `.1–.49` reserviert, DHCP-Pool `.100–.200`.
  * **DHCP-Reservierung:** Ein bestimmtes Gerät erhält anhand seiner **MAC-Adresse** immer dieselbe IP aus dem DHCP-Server (zentral pflegbar!).
  * **Lease-Verlängerung:** Nach $50\,\%$ der Lease-Zeit ($T_1$) fragt der Client per Unicast nach Verlängerung; bei Ausfall des Servers nach $87{,}5\,\%$ ($T_2$) per Broadcast.

---

### 2. DNS (*Domain Name System* – UDP/TCP Port 53)
Übersetzt menschenlesbare Namen (`housekeeping.hotel-arnsee.intern`) in IP-Adressen – unerlässlich für TLS-Zertifikate, Dual-Stack und wartbare Software!

| Record-Typ | Funktion | Beispiel |
| :---: | :--- | :--- |
| **`A`** | Hostname $\rightarrow$ **IPv4-Adresse** | `app.seeblick.intern. IN A 10.20.40.10` |
| **`AAAA`** | Hostname $\rightarrow$ **IPv6-Adresse** | `app.seeblick.intern. IN AAAA 2001:db8:40::10` |
| **`CNAME`** | **Alias** auf einen anderen Hostnamen | `www.seeblick.de. IN CNAME app.seeblick.de.` |
| **`PTR`** | **Reverse-DNS:** IP-Adresse $\rightarrow$ Hostname | `10.40.20.10.in-addr.arpa. IN PTR app.seeblick.intern.` |
| **`MX`** | Zuständiger **Mailserver** (mit Priorität) | `seeblick.de. IN MX 10 mail.seeblick.de.` |
| **`TXT`** | Freitext (z. B. **SPF, DKIM, DMARC**, Domain-Verifizierung) | `v=spf1 mx -all` |
| **`NS` / `SOA`** | Zuständiger Nameserver / Zonen-Startinformationen | Zonenverwaltung |

* **Diagnose-Befehle:**
  * `nslookup app.seeblick.intern` bzw. PowerShell: `Resolve-DnsName app.seeblick.intern -Type AAAA`
  * DNS-Cache leeren: `ipconfig /flushdns`

---

### 🎯 Meilenstein (Modul 5)
* Vollständiges **DHCP- & DNS-Konzept** (Adress-Pool, Reservierungen, Forward-/Reverse-Zone für die eigene Web-Anwendung) inkl. Funktionstest-Nachweis.
* *(Querverweis 1. LJ: [LF 3 – Teil 6](../../1%20-%20LJ/03_LF3_Clients_in_Netzwerke_einbinden/06_Netzwerkdienste_TCP_UDP_Ports_DHCP_DNS_NAT_Diagnose.md))*
