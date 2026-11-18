# 🔐 LF 4 & BfK-I LS 12: Netzwerksicherheit (Firewall, DMZ, VPN) & Kryptografie (Verschlüsselung, Signatur, Zertifikate)

> **Lernfeld 4 & BfK-I LS 12:** IT-Security durch Firewall, DMZ, VPN und Kryptografie  
> **AP1-Statistik:** *Verschlüsselung (10 P.), VPN (7 P.), Hashverfahren & Zertifikate (7 P.)* = **24 Punkte** in der AP1 (und 48 Punkte in der AP2)!

---

## ✅ Checkliste der Lehrinhalte (LS 12)
- [ ] **Firewall-Typen** (Paketfilter, Stateful Inspection, Application-Layer-Firewall / Proxy, NGFW) unterscheiden
- [ ] Aufbau und Zweck einer **DMZ (Demilitarisierte Zone – einstufig vs. zweistufig)** erklären
- [ ] **VPN-Arten** (End-to-Site, Site-to-Site, End-to-End) zuordnen
- [ ] **Symmetrische vs. Asymmetrische vs. Hybride Verschlüsselung** vergleichen
- [ ] Ablauf einer **Digitalen Signatur** und Rolle einer **Zertifizierungsstelle (CA / PKI)** Schritt für Schritt erklären

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Firewall-Architekturen & Demilitarisierte Zone (DMZ)

* **Firewall-Typen nach OSI-Schichten:**
  1. **Paketfilter (*Stateless Packet Filter* – Layer 3 & 4):** Prüft jedes einzelne Paket isoliert anhand starrer Regeln für **Quell-/Ziel-IP-Adresse, Protokoll (TCP/UDP/ICMP) und Quell-/Ziel-Port**. Kennt keinen Verbindungszustand.
  2. **Stateful Inspection Firewall (Layer 3 & 4 mit Zustandstabelle):** Merkt sich aktive ausgehende Verbindungen in einer *State Table*: Antwortpakete zu einer von innen angeforderten Verbindung werden automatisch durchgelassen, unangeforderte Pakete von außen werden blockiert.
  3. **Application-Layer-Firewall / Proxy / WAF (Layer 7):** Analysiert den **Inhalt (Payload)** des Anwendungsprotokolls (z. B. HTTP-Befehle), erkennt SQL-Injections, Viren oder verbotene Webseiten.
  4. **Next-Generation Firewall (NGFW):** Kombiniert Stateful Firewall, Application Control, **IDS/IPS** (*Intrusion Detection / Prevention System*), Deep Packet Inspection (DPI) und VPN-Gateway.

* **DMZ (*Demilitarisierte Zone* – Schutzpuffer für öffentlich erreichbare Server):**
  * **Problem:** Ein Firmen-Webserver oder Mailserver muss aus dem unsicheren Internet erreichbar sein. Stellt man ihn direkt ins interne LAN, hätte ein Hacker bei einer Sicherheitslücke im Webserver sofort Zugriff auf alle internen Buchhaltungs-PCs!
  * **Lösung (Zweistufige DMZ mit 2 Firewalls):**

```text
[ Unsicheres INTERNET ]
          |
   +--------------+
   | Äußere FW 1  |  (Erlaubt aus dem Internet NUR Port 80/443 zum Webserver in der DMZ!)
   +--------------+
          |
=== [ DMZ-Netz ] ===---> [ Öffentlicher Webserver / Mailserver / Reverse Proxy ]
          |
   +--------------+
   | Innere FW 2  |  (Blockiert direkten Zugriff vom Internet ins LAN komplett!
   +--------------+   Erlaubt nur streng geprüfte Einzelverbindungen zwischen DMZ und LAN)
          |
=== [ Internes LAN ] ===---> [ Interne Clients, Datenbankserver, Active Directory ]
```

---

### 2. VPN (*Virtual Private Network* – AP1: 27 % Häufigkeit!)
Erzeugt einen **verschlüsselten, abhörsicheren und authentifizierten „Tunnel“** durch ein unsicheres öffentliches Netzwerk (das Internet):
1. **End-to-Site VPN (*Remote-Access-VPN*):** Ein einzelnes mobiles Endgerät (Laptop im Homeoffice/Hotel) verbindet sich über ein VPN-Gateway (z. B. OpenVPN, WireGuard, IPSec) mit dem Firmennetzwerk und erhält eine virtuelle interne Firmen-IP.
2. **Site-to-Site VPN (*LAN-to-LAN-Kopplung*):** Zwei feste Firmenstandorte (z. B. Zentrale in Stuttgart und Filiale in München) verbinden ihre beiden VPN-Router dauerhaft über das Internet miteinander, sodass beide Netze wie ein einziges gemeinsames LAN arbeiten.
3. **End-to-End VPN (*Host-to-Host*):** Direkter verschlüsselter Tunnel zwischen genau zwei Endgeräten (z. B. Admin-PC direkt zu einem sensiblen Server).

---

### 3. Kryptografie: Symmetrisch vs. Asymmetrisch vs. Hybrid

| Kriterium | Symmetrische Verschlüsselung | Asymmetrische Verschlüsselung | Hybride Verschlüsselung |
| :--- | :--- | :--- | :--- |
| **Schlüsselprinzip** | **1 einziger geheimer Schlüssel (*Secret Key*)** zum Verschlüsseln **und** Entschlüsseln. | **Schlüsselpaar (2 Schlüssel)** pro Teilnehmer:<br>1. **Public Key (Öffentlich):** darf jeder haben.<br>2. **Private Key (Privat):** streng geheim! | Kombination aus beiden: **Asymmetrischer Schlüsselaustausch** + **symmetrische Nutzdatenverschlüsselung**. |
| **Wichtige Algorithmen** | **AES (128/256 Bit)**, ChaCha20 | **RSA (2048/4096 Bit)**, **ECC** (Elliptische Kurven) | **TLS / HTTPS**, PGP, S/MIME, SSH |
| **Geschwindigkeit** | **Extrem schnell** (ideal für große Datenmengen, Festplatten, Videostreams). | **Sehr langsam / rechenintensiv** ($\approx 1.000\text{-mal}$ langsamer als AES). | **Sehr schnell** (da die großen Nutzdaten mit AES verschlüsselt werden). |
| **Hauptvorteil / Hauptnachteil** | *Nachteil:* **Schlüsselaustauschproblem** (Wie kommt der geheime Schlüssel sicher zum Partner?). | *Vorteil:* Löst das Schlüsselaustauschproblem, da nur der unkritische Public Key verschickt wird! | Vereint die Sicherheit von Asymmetrisch mit dem Tempo von Symmetrisch! |

> **Goldene Merkregel für Asymmetrische Verschlüsselung vs. Digitale Signatur in der AP1:**
> * **VERSCHLÜSSELN (Schutzziel: Vertraulichkeit – Geheimhaltung):**  
>   Sender verschlüsselt die Nachricht mit dem **PUBLIC KEY des Empfängers** $\longrightarrow$ Nur der **Empfänger** kann sie mit seinem **PRIVATE KEY** entschlüsseln!
> * **DIGITAL SIGNIEREN (Schutzziele: Integrität, Authentizität & Verbindlichkeit):**  
>   Absender berechnet den **Hashwert (z. B. SHA-256)** der Nachricht und verschlüsselt diesen Hashwert mit seinem **EIGENEN PRIVATE KEY** (= Digitale Signatur) $\longrightarrow$ Jeder Empfänger kann die Signatur mit dem **PUBLIC KEY des Absenders** entschlüsseln und prüfen, ob der Hashwert mit dem neu berechneten Hashwert der Nachricht übereinstimmt!

* **Rolle von Zertifikaten (X.509) & Zertifizierungsstellen (CA / PKI):**
  * Damit ein Angreifer (*Man-in-the-Middle*) dem Empfänger keinen gefälschten Public Key unterjubeln kann, beglaubigt eine vertrauenswürdige **Zertifizierungsstelle (CA – *Certificate Authority*, z. B. Let's Encrypt, DigiCert)** durch ein **digitales Zertifikat**, dass ein bestimmter Public Key tatsächlich zu einer bestimmten Domain/Person gehört.
