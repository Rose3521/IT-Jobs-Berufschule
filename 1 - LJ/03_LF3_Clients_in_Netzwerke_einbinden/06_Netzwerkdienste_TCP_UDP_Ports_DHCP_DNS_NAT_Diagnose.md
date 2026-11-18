# 🌐 BfK-I (LF 3) – LS 11: Netzwerkdienste (TCP vs. UDP, Ports, DHCP, DNS, NAT & Diagnose)

> **Fach:** BfK-I (it.schule Stuttgart – im Screenshot grün markiert für AP1!) | **Lernfeld 3:** Clients in Netzwerke einbinden  
> **AP1-Statistik:** *`ping` (11 P.), Protokolle (7 P.), DHCP (3 P.)* kommen regelmäßig in der AP1 vor!

---

## ✅ Checkliste der Lehrinhalte (LS 11)
- [ ] **TCP vs. UDP** (Layer 4: 3-Way-Handshake, Zuverlässigkeit vs. Echtzeit/Geschwindigkeit) vergleichen
- [ ] Wichtigste **Well-Known Ports** (HTTP, HTTPS, SSH, FTP, DNS, DHCP, SMTP, IMAP, POP3, RDP) zuordnen
- [ ] **DHCP** (Automatische IP-Konfiguration nach dem **DORA-Prinzip**) erklären
- [ ] **DNS** (Namensauflösung & Record-Typen: `A`, `AAAA`, `CNAME`, `MX`, `PTR`) erläutern
- [ ] **NAT / PAT** am Internet-Router sowie systematische Fehlerdiagnose mit **`ping`, `ipconfig`, `tracert`, `nslookup`** durchführen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Transportschicht (Layer 4): TCP vs. UDP & Standard-Ports

| Eigenschaft | TCP (*Transmission Control Protocol*) | UDP (*User Datagram Protocol*) |
| :--- | :--- | :--- |
| **Verbindungsart** | **Verbindungsorientiert** (**3-Way-Handshake:** `SYN` $\rightarrow$ `SYN+ACK` $\rightarrow$ `ACK`) | **Verbindungslos** (*Fire and Forget*) |
| **Sicherung & Kontrolle** | Empfangsbestätigungen (`ACK`), Neuübertragung verlorener Pakete, korrekte Reihenfolge, Flusskontrolle. | **Keine** Empfangsbestätigung, keine erneute Anforderung verlorener Pakete. |
| **Geschwindigkeit / Latenz** | Langsamer (größerer 20-Byte-Header + Handshake-Overhead). | **Extrem schnell / minimale Latenz** (schlanker 8-Byte-Header). |
| **Einsatzgebiete** | **HTTP/HTTPS, E-Mail (SMTP/IMAP/POP3), Dateitransfer (FTP/SMB), SSH, RDP** | **VoIP-Telefonie, Videokonferenzen/Livestreams, Online-Gaming, DNS, DHCP** |

#### Die wichtigsten Standard-Ports für die AP1
* **`20 / 21` (TCP):** **FTP** (File Transfer Protocol – unverschlüsselt!)
* **`22` (TCP):** **SSH / SFTP** (Secure Shell – verschlüsselte Fernwartung & Dateitransfer)
* **`23` (TCP):** **Telnet** (unverschlüsselte Text-Fernwartung – unsicher!)
* **`25` (TCP):** **SMTP** (Simple Mail Transfer Protocol – E-Mail-Versand)
* **`53` (UDP/TCP):** **DNS** (Domain Name System – Namensauflösung)
* **`67 / 68` (UDP):** **DHCP** (Server Port 67, Client Port 68)
* **`80` (TCP):** **HTTP** (Webseiten unverschlüsselt)
* **`110` / `995` (TCP):** **POP3 / POP3S** (E-Mail-Abruf vom Server)
* **`143` / `993` (TCP):** **IMAP / IMAPS** (E-Mail-Synchronisation auf dem Server)
* **`443` (TCP/UDP):** **HTTPS** (Webseiten TLS-verschlüsselt)
* **`3389` (TCP/UDP):** **RDP** (Microsoft Remote Desktop Protocol)

---

### 2. DHCP, DNS & NAT / PAT

#### A. DHCP (*Dynamic Host Configuration Protocol*) – Das DORA-Prinzip
Vergibt automatisch **IP-Adresse, Subnetzmaske, Standardgateway, DNS-Server und Lease-Time**:
1. **D – Discover (Broadcast):** Client ruft ins Netz: *„Gibt es hier einen DHCP-Server?“*
2. **O – Offer:** DHCP-Server bietet eine freie IP-Adresse aus seinem Pool an.
3. **R – Request (Broadcast):** Client nimmt das Angebot verbindlich an.
4. **A – Acknowledge:** DHCP-Server bestätigt die Vergabe für die Dauer der *Lease-Time*.
*(Ist ein Router zwischen Client und zentralem DHCP-Server, blockiert der Router normalerweise den Broadcast $\rightarrow$ man muss auf dem Router einen **DHCP-Relay-Agent (`ip helper-address <IP-des-DHCP-Servers>`)** einrichten, der den Broadcast als Unicast an den DHCP-Server weiterleitet!)*

#### B. DNS (*Domain Name System*) – Das Telefonbuch des Internets
Übersetzt menschenlesbare Domainnamen (z. B. `www.ihk.de`) in IP-Adressen (und umgekehrt):
* **`A`-Record:** Domainname $\rightarrow$ **IPv4-Adresse**
* **`AAAA`-Record:** Domainname $\rightarrow$ **IPv6-Adresse**
* **`CNAME`-Record:** Alias-Name $\rightarrow$ verweist auf einen anderen Domainnamen
* **`MX`-Record (*Mail Exchange*):** Zuständiger **E-Mail-Server** einer Domain
* **`PTR`-Record (*Pointer*):** **Reverse-DNS** (IP-Adresse $\rightarrow$ Domainname)

#### C. NAT / PAT (*Network / Port Address Translation*)
* Da **private IPv4-Adressen** (z. B. `192.168.1.x`) im öffentlichen Internet nicht geroutet werden, ersetzt der Internet-Router beim Senden ins Internet die private Quell-IP jedes PCs durch seine **einzige öffentliche WAN-IPv4-Adresse** und merkt sich in einer **NAT-Übersetzungstabelle anhand der Quell-Portnummer (PAT)**, zu welchem internen PC die eintreffende Antwort gehört.

---

### 3. Systematische Fehlerdiagnose mit `ping`, `ipconfig` & `nslookup` (AP1: 11 Punkte!)

Wenn in einer Prüfungsaufgabe steht *„PC A erreicht die Webseite `www.firma.de` nicht – wie gehen Sie bei der Fehlersuche vor?“*:
1. **`ipconfig /all` prüfen:** Hat der PC eine gültige IP, Subnetzmaske, Standardgateway und DNS-Server – oder eine **`169.254.x.x` (APIPA)**? *(Falls APIPA: DHCP-Server nicht erreichbar oder Netzwerkkabel/VLAN-Port falsch!).*
2. **`ping 127.0.0.1`:** Funktioniert das eigene TCP/IP-Protokoll?
3. **`ping <Standardgateway-IP>` (z. B. `192.168.1.1`):** Ist der eigene Router im LAN erreichbar?
4. **`ping 8.8.8.8` (öffentliche IP im Internet):** Funktioniert das Routing/NAT ins Internet?
5. **`ping www.firma.de` / `nslookup www.firma.de`:** Klappt der Ping auf die IP `8.8.8.8`, aber **nicht** auf den Namen `www.firma.de`, liegt ein **DNS-Fehler** vor!
