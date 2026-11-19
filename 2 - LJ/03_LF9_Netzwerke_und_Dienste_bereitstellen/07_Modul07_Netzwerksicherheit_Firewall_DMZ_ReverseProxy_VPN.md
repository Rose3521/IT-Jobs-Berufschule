# 🛡️ LF 9 – Modul 7: Netzwerksicherheit & Fernzugriff (Firewall, DMZ, Reverse Proxy & VPN)

> **Lernsituation:** *Ingenieurbüro Hartwig & Partner* (internes Projektportal sicher für Außendienst & ausgewählte Bauherren erreichbar machen, ohne interne Konstruktionsdaten offenzulegen)  
> **EduSysTrainings-Status:** 🟡 Einheiten `07-01` bis `07-04` im Lehrerprojekt noch 🚧 *In Vorbereitung* – hier das prüfungsrelevante Kernwissen als Arbeitsgrundlage.

---

## ✅ Checkliste der Lehrinhalte (Modul 7)
- [ ] **Firewall-Paketfilterregeln** (Quelle, Ziel, Protokoll/Port, Aktion, Reihenfolge / *First-Match* & **Default Deny**) korrekt lesen, schreiben und prüfen
- [ ] **Stateless vs. Stateful Inspection** (Verbindungstabelle, Initiator-Prinzip) unterscheiden
- [ ] **DMZ (*Demilitarized Zone*)** und **Reverse Proxy** (TLS-Terminierung, Verstecken interner Server, zentrale Filterung/WAF) architektonisch einsetzen
- [ ] **VPN-Konzepte** (*End-to-Site / Remote-Access* für Außendienst vs. *Site-to-Site* für Standorte; Protokolle **WireGuard, IPSec, OpenVPN**) begründet auswählen
- [ ] **Sicherheitsanforderungen an Web-Anwendungen** (Least Privilege, HTTPS/TLS, Authentifizierung/MFA, Logging, Schutz vor OWASP-Risiken) ableiten

---

## 📌 Kernwissen & Prüfungsgrundlagen

### 1. Firewall-Regelwerk (Goldene Regeln)
Eine Stateful-Firewall arbeitet ihre Regeltabelle **von oben nach unten** ab (**First-Match-Prinzip**). Am Ende steht immer eine implizite oder explizite **`DENY ALL`-Regel (Whitelisting / Default Deny)**!

| Nr. | Quelle | Ziel | Protokoll / Port | Aktion | Zweck |
| :---: | :--- | :--- | :--- | :---: | :--- |
| 1 | `Internet (any)` | `DMZ-ReverseProxy` | `TCP / 443` | **ALLOW** | Bauherren greifen per HTTPS auf Portal zu |
| 2 | `DMZ-ReverseProxy` | `LAN-AppServer` | `TCP / 8443` | **ALLOW** | Proxy leitet geprüfte Anfragen ans interne Backend weiter |
| 3 | `DMZ-ReverseProxy` | `LAN-Konstruktion` | `any` | **DENY** | Kein direkter Zugriff aus der DMZ auf interne Konstruktionsdaten! |
| 4 | `any` | `any` | `any` | **DENY** | **Default Deny** (alles nicht ausdrücklich Erlaubte blockieren) |

---

### 2. DMZ & Reverse Proxy
* **DMZ (Zweistufig empfohlen):** Pufferzone zwischen Internet und internem LAN. Wird ein öffentlich erreichbarer Server in der DMZ kompromittiert, bleibt das interne LAN durch die innere Firewall geschützt (**keine direkte Verbindung von der DMZ zur internen Datenbank/Konstruktionsablage!**).
* **Reverse Proxy (z. B. Nginx, Traefik, Caddy):** Steht vor den eigentlichen Anwendungsservern, beendet **TLS/HTTPS** zentral, verbirgt interne Server-IPs/Ports, übernimmt ggf. Load-Balancing, Rate-Limiting und Header-Härtung.

---

### 3. VPN (*Virtual Private Network*)
Erstellt einen **verschlüsselten Tunnel** über ein unsicheres öffentliches Netz (Internet):
* **End-to-Site (Remote-Access-VPN):** Für Mitarbeitende im Außendienst / Homeoffice $\rightarrow$ sicherer Zugang zu internen Ressourcen ohne Portweiterleitungen auf SSH/RDP/Dateiserver.
* **Site-to-Site-VPN:** Dauerhafte verschlüsselte Kopplung zweier Firmenstandorte (Router-zu-Router).
* **Typische Protokolle:** **WireGuard** (modern, extrem schnell, schlank), **IPSec / IKEv2** (Industriestandard auf Schicht 3), **OpenVPN** (TLS-basiert, flexibel).

---

### 🎯 Meilenstein (Modul 7)
* Dokumentiertes **Sicherheits- & Fernzugriffskonzept** (Firewall-Regeltabelle, DMZ/Reverse-Proxy-Skizze, VPN-Auswahl & Schutzmaßnahmen für interne Konstruktionsdaten).
* *(Querverweis 1. LJ: [LF 4 – Teil 5](../../1%20-%20LJ/04_LF4_Schutzbedarfsanalyse_und_IT_Sicherheit/05_Firewall_DMZ_VPN_Kryptografie_und_Digitale_Signatur.md))*
