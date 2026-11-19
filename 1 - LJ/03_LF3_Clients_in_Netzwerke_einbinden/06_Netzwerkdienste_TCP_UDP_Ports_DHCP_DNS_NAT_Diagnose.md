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

#### C. NAT / PAT (*Source-NAT*) & Portweiterleitung (*Destination-NAT*) im Detail *(Erweitert aus EduSysTrainings)*

* **Source-NAT mit Portumsetzung (PAT / NAPT – Weg von innen ins Internet):**
  * Da private IPv4-Adressen (RFC 1918: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) im Internet nicht geroutet werden, ersetzt der Router auf dem **Hinweg** die private Absender-IP und den Quellport durch seine **öffentliche WAN-IP + einen freien öffentlichen Port** und speichert diese Zuordnung in seiner **NAT-Tabelle**:
    ```text
    Hinweg:  PC (10.20.10.23:51544) ---> [NAT-Router: 203.0.113.42:40000] ---> Webserver (198.51.100.80:443)
    Rückweg: PC (10.20.10.23:51544) <--- [NAT-Router schlägt 40000 nach!] <--- Webserver (198.51.100.80:443)
    ```
  * *Nebeneffekt:* Unangeforderte Pakete von außen passen zu keinem Eintrag in der NAT-Tabelle und werden verworfen. **Achtung:** NAT ersetzt trotzdem **keine Firewall**, da es ausgehenden Verkehr und internen Verkehr nicht kontrolliert!
* **Portweiterleitung (*Port Forwarding* / Destination-NAT / DNAT – Weg von außen nach innen):**
  * Feste Regel im Router, damit ein eigener Server (z. B. Web-App) aus dem Internet erreichbar ist: `203.0.113.42:443` $\rightarrow$ `10.20.40.10:443` (hier wird die **Ziel-IP** umgeschrieben; der Server sieht die echte Kunden-IP als Absender).
  * **⚠️ Sicherheitsregeln für Portweiterleitungen:**
    1. **So wenige wie möglich** (nur Dienste, die wirklich für externe Nutzer gedacht sind, z. B. HTTPS Port `443`).
    2. **NIEMALS Datenbanken (`5432`, `3306`) oder Admin-Zugänge (`22` SSH, `3389` RDP) per Portweiterleitung ins Internet öffnen!** Für interne Zugriffe aus dem Homeoffice immer ein **VPN** nutzen!
    3. **„Security by Obscurity“ schützt nicht:** Einen Dienst auf einen ungewöhnlichen Port (z. B. `2222` oder `8443`) zu legen, hält automatisierte Port-Scanner nur Sekunden auf.
* **Die 4 typischen Probleme durch NAT in Anwendungen (Praxis-Gold!):**

| NAT-Effekt | Was passiert technisch? | Gegenmaßnahme / Lösung |
| :--- | :--- | :--- |
| **1. Leerlauf-Timeout** | Läuft über eine TCP-Verbindung z. B. 300 s lang kein Datenpaket, löscht der Router den NAT-Eintrag $\rightarrow$ Live-Verbindung (z. B. Chat/Termin-Ansicht) hängt stillschweigend! | Anwendung sendet regelmäßig ein kleines Lebenszeichen (**Heartbeat / Keep-Alive**) deutlich kürzer als der Timeout und baut bei Abbruch automatisch neu auf. |
| **2. Viele Nutzer teilen sich 1 IP (*CGNAT*)** | Alle 25 Mitarbeiter einer Firma (oder tausende Mobilfunknutzer bei *Carrier-Grade-NAT*) erscheinen beim Server mit derselben öffentlichen IP! | **Login-Sperren / Rate-Limits niemals allein an die IP-Adresse koppeln** (sonst sperrt 1 sich vertippender Mitarbeiter die gesamte Firma aus!), sondern primär ans **Benutzerkonto**! |
| **3. NAT-Loopback (*Hairpinning*)** | Ein PC im internen Firmennetz ruft die öffentliche Domain/IP der eigenen Firma auf; viele Router können das Paket nicht „umdrehen“ und zurück ins LAN schicken $\rightarrow$ Seite lädt nicht! | **Internes DNS (*Split-Horizon DNS*):** Intern wird der Domainname direkt auf die interne Server-IP aufgelöst, extern auf die öffentliche IP. |
| **4. Von außen nicht erreichbar** | Ein Client hinter NAT kann keine eingehenden Verbindungen annehmen. | Der Client baut die Verbindung selbst **von innen nach außen** zum Server auf und hält sie offen (Prinzip von Push-Diensten & Messengern). |

---

### 3. Systematische Fehlerdiagnose mit `ping`, `ipconfig`, `Test-NetConnection` & `nslookup` (AP1: 11 Punkte!)

Wenn in einer Prüfungsaufgabe steht *„PC A erreicht die Webseite `www.firma.de` nicht – wie gehen Sie bei der Fehlersuche vor?“*:
1. **`ipconfig /all` prüfen:** Hat der PC eine gültige IP, Subnetzmaske, Standardgateway und DNS-Server – oder eine **`169.254.x.x` (APIPA)**? *(Falls APIPA: DHCP-Server nicht erreichbar oder Netzwerkkabel/VLAN-Port falsch!).*
2. **`ping 127.0.0.1`:** Funktioniert das eigene TCP/IP-Protokoll?
3. **`ping <Standardgateway-IP>` (z. B. `192.168.1.1`):** Ist der eigene Router im LAN erreichbar?
4. **`ping 8.8.8.8` (öffentliche IP im Internet):** Funktioniert das Routing/NAT ins Internet?
5. **`ping www.firma.de` / `nslookup www.firma.de`:** Klappt der Ping auf die IP `8.8.8.8`, aber **nicht** auf den Namen `www.firma.de`, liegt ein **DNS-Fehler** vor!
