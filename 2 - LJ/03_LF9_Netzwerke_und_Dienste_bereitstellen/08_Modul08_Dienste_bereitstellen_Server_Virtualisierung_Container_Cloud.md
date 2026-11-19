# ☁️ LF 9 – Modul 8: Dienste bereitstellen – Server, Virtualisierung, Container & Cloud

> **Lernsituation:** *TSV Grünwiesen 1908 e. V.* (Mitgliederverwaltung als Web-App: eigener Server im Vereinsheim, Hoster oder Cloud?)  
> **EduSysTrainings-Status:** 🟡 Einheiten `08-01` bis `08-03` im Lehrerprojekt noch 🚧 *In Vorbereitung* – hier das prüfungsrelevante Kernwissen als Arbeitsgrundlage.

---

## ✅ Checkliste der Lehrinhalte (Modul 8)
- [ ] **Bereitstellungsmodelle** (*On-Premise / eigener Server, Virtualisierung Typ 1/2, Container/Docker, Cloud IaaS/PaaS/SaaS*) nach Betrieb, Wartungsaufwand & Kontrolle vergleichen
- [ ] **Virtuelle Maschinen (VMs) vs. Container (Docker)** architektonisch (Hypervisor + Gast-OS vs. geteilter Host-Kernel) unterscheiden
- [ ] Eine Anwendung im **Container mit Netzwerk, Port-Mapping (`-p`), Umgebungsvariablen (`-e`) und Volumes (`-v`)** bereitstellen
- [ ] Bereitstellungsvarianten systematisch nach **Kosten (CAPEX vs. OPEX), Datenschutz (DSGVO / AVV / Serverstandort) und Verfügbarkeit** bewerten (Nutzwertanalyse!)

---

## 📌 Kernwissen & Prüfungsgrundlagen

### 1. Bereitstellungsmodelle im Vergleich (Shared Responsibility)

| Modell | Was verwaltest DU? | Was verwaltet der Anbieter? | Typisches Beispiel |
| :--- | :--- | :--- | :--- |
| **On-Premise (Eigener Server)** | Alles (Hardware, Strom, USV, OS, Backups, App, Sicherheit) | Nichts | Server im Vereinsheim / eigenen Serverraum |
| **IaaS** *(Infrastructure as a Service)* | OS, Patches, Laufzeit, App, Daten | Rechenzentrum, Hardware, Virtualisierung | Virtueller Server (vServer / EC2 / Hetzner Cloud) |
| **PaaS** *(Platform as a Service)* | Deine Anwendung & Daten | Hardware, OS, Laufzeitumgebung, Updates | Managed App-Hosting, Managed Database |
| **SaaS** *(Software as a Service)* | Konfiguration & Nutzerdaten | Kompletten Betrieb & Software | Microsoft 365, Cloud-Vereinssoftware |

---

### 2. Virtuelle Maschinen (VM) vs. Container (Docker)
* **VM (Hypervisor Typ 1 Bare-Metal wie Proxmox/ESXi vs. Typ 2 Hosted):** Jede VM enthält ein **vollständiges eigenes Gast-Betriebssystem** $\rightarrow$ starke Isolation, aber höherer RAM-/Speicherbedarf und längere Startzeit.
* **Container (z. B. Docker / Podman):** Teilen sich den **Kernel des Host-Betriebssystems**, isolieren Prozesse/Dateisystem/Netzwerk über Namespaces & cgroups $\rightarrow$ sekundenschneller Start, reproduzierbare Images (*„funktioniert auf jedem Rechner identisch“*).
* **Zentrale Docker-Bausteine für den Netzbetrieb:**
  * **Port-Mapping (`-p 127.0.0.1:8080:80`):** Verknüpft Host-Port `8080` mit Container-Port `80` *(Bind an `127.0.0.1`, wenn ein Reverse Proxy davor sitzt!)*.
  * **Docker-Netzwerk (`docker network create`):** App-Container und DB-Container sprechen über Containernamen (internes DNS) miteinander – die DB bekommt **kein** Port-Mapping nach außen!
  * **Volumes (`-v`):** Persistente Datenspeicherung (Daten überleben Neustart/Update des Containers!).

---

### 3. Entscheidungsmatrix: Kosten, DSGVO & Verfügbarkeit
* **Kosten:** Eigener Server = hohe Einmalinvestition (**CAPEX**) + Strom + Wartungszeit vs. Hoster/Cloud = planbare laufende Kosten (**OPEX**).
* **Datenschutz (DSGVO):** Bei externem Hosting zwingend **Auftragsverarbeitungsvertrag (AVV, Art. 28 DSGVO)**, Serverstandort in der **EU/EWR** (keine ungesicherte Drittlandübermittlung), Verschlüsselung & Backup-Konzept prüfen.
* **Verfügbarkeit:** Vereinsheim-Anschluss hat oft keine feste IP, keinen redundanten Strom/USV und geringen Upload $\rightarrow$ professionelles EU-Rechenzentrum meist deutlich zuverlässiger!

---

### 🎯 Meilenstein (Modul 8)
* Begründete **Entscheidungsvorlage / Nutzwertanalyse** (*On-Premise vs. Hoster vs. Cloud/Container*) inkl. Datenschutz- und Betriebskonzept.
* *(Querverweis 1. LJ: [LF 2 – Teil 2 (Virtualisierung & Cloud)](../../1%20-%20LJ/02_LF2_Arbeitsplaetze_nach_Kundenwunsch_ausstatten/02_Betriebssysteme_Dateisysteme_Virtualisierung_und_Cloud.md) & [BWL – Teil 5 (Nutzwertanalyse)](../../1%20-%20LJ/07_BWL_Projektmanagement_Vertragsrecht_und_Kalkulation/05_Nutzwertanalyse_Kalkulation_Leasing_SLA_und_4_Ohren_Modell.md))*
