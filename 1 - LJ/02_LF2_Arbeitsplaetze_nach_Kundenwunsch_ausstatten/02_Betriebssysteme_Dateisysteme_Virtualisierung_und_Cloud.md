# ☁️ LF 2 – Betriebssysteme, Dateisysteme, Lizenzmodelle, Virtualisierung (LS 8) & Cloud

> **Lernfeld 2 & BfK-I LS 8:** Arbeitsplätze ausstatten / Virtualisierte Server für IT & OT bereitstellen  
> **AP1-Statistik:** *Dateisysteme, Lizenzen, Open Source, Cloud & Betriebssysteme* kamen in mehreren AP1-Prüfungen (2022F, 2023F, 2025F, 2026H) vor!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Dateisysteme (FAT32, exFAT, NTFS, ext4)** nach Dateigrößenlimit und Features vergleichen
- [ ] **Lizenzmodelle** (Proprietär vs. Open Source, GPL vs. MIT, OEM, Volumenlizenz, CAL) unterscheiden
- [ ] **Virtualisierung (LS 8):** **Typ-1- vs. Typ-2-Hypervisor** sowie **VM vs. Container (Docker)** erklären
- [ ] **Cloud Computing:** **IaaS, PaaS, SaaS** sowie **Public, Private, Hybrid Cloud** abgrenzen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Dateisysteme im Überblick (AP1 Herbst 2026)

| Dateisystem | Max. Dateigröße | Max. Partitionsgröße | Eigenschaften & Einsatzgebiet |
| :--- | :--- | :--- | :--- |
| **FAT32** | **Max. 4 GiB** ($- 1\text{ Byte}$) | $2\text{ TiB}$ (bis $16\text{ TiB}$) | Universell kompatibel, aber **keine** Zugriffsrechte (ACLs), keine Verschlüsselung, kein Journaling. *(AP1-Klassiker: „Warum lässt sich ein 6 GB großes Backup-Image nicht auf den leeren 32-GB-USB-Stick kopieren?“ $\rightarrow$ Weil er mit FAT32 formatiert ist!)* |
| **exFAT** | $16\text{ EiB}$ (quasi unbegrenzt) | $128\text{ PiB}$ | Moderner Standard für USB-Sticks und SD-Karten $>32\text{ GB}$; unterstützt Dateien $>4\text{ GiB}$ unter Windows, macOS und Linux. |
| **NTFS** | $16\text{ TiB}$ (theor. $16\text{ EiB}$) | $256\text{ TiB}$ | Standard für **Windows-Systemlaufwerke**; bietet **Journaling** (Schutz vor Datenverlust bei Absturz), feingranulare **Zugriffsrechte (ACLs)**, Verschlüsselung (EFS), Komprimierung und Schattenkopien. |
| **ext4** | $16\text{ TiB}$ | $1\text{ EiB}$ | Standard-Dateisystem unter **Linux**; bietet Journaling, POSIX-Rechte und hohe Stabilität. |

---

### 2. Software-Lizenzmodelle (AP1 2025F, 2026H)
* **Proprietäre Software (*Closed Source*):** Quellcode ist geheim; der Kunde kauft nur ein eingeschränktes **Nutzungsrecht (EULA)**, darf die Software nicht verändern oder weiterverkaufen (z. B. Microsoft Windows, Adobe Photoshop).
* **Open-Source-Software (Quelloffen):** Quellcode ist frei einsehbar, darf verändert und weitergegeben werden. Wichtiger Unterschied bei den Lizenzen:
  * **Strenges Copyleft (z. B. GNU GPL – Linux-Kernel):** Wer GPL-Code verändert und seine Software veröffentlicht/vertreibt, **muss seinen gesamten abgeleiteten Quellcode ebenfalls wieder kostenlos unter der GPL offenlegen** (*Virale Lizenz*)!
  * **Permissive / Freizügige Lizenzen (z. B. MIT, Apache, BSD):** Erlauben es, den Open-Source-Code auch in **kommerzielle Closed-Source-Produkte** einzubauen, ohne den eigenen Code veröffentlichen zu müssen (solange der Urheberhinweis erhalten bleibt).
* **Unternehmens-Lizenzierungsarten:**
  * **Named-User-Lizenz:** Gebunden an eine bestimmte natürliche Person.
  * **Concurrent-User-Lizenz (Floating License):** Begrenzt die Anzahl der **gleichzeitig** eingeloggten Nutzer (z. B. 50 Mitarbeiter im Schichtbetrieb teilen sich 15 gleichzeitige Lizenzen).
  * **CAL (*Client Access License*):** Zugriffslizenz bei Server-Betriebssystemen (z. B. Windows Server: *User-CAL* pro Mitarbeiter oder *Device-CAL* pro Endgerät).

---

### 3. Virtualisierung (LS 8): Hypervisor Typ 1 vs. Typ 2 & Container

* **Typ-1-Hypervisor (*Bare-Metal / Native*):**
  * Wird **direkt auf die nackte Server-Hardware** installiert – es gibt kein darunterliegendes Host-Betriebssystem!
  * *Vorteil:* Minimaler Overhead, maximale Performance, Stabilität und Sicherheit im Rechenzentrum.
  * *Beispiele:* **VMware ESXi (vSphere), Proxmox VE (KVM), Microsoft Hyper-V Server, Citrix XenServer**.
* **Typ-2-Hypervisor (*Hosted*):**
  * Läuft als normales Anwendungsprogramm **auf einem bestehenden Host-Betriebssystem** (Windows, macOS, Ubuntu Desktop).
  * *Vorteil:* Ideal für Entwickler, Schulungen und Testumgebungen auf dem eigenen Arbeitsplatz-PC.
  * *Beispiele:* **Oracle VirtualBox, VMware Workstation / Player**.
* **Virtuelle Maschine (VM) vs. Container (z. B. Docker):**
  * **VM:** Virtualisiert komplette Hardware; jede VM bootet ihr **eigenes vollständiges Gast-Betriebssystem (inkl. eigenem Kernel)** $\rightarrow$ groß (viele GB), langsamer Start, aber starke Sicherheitstrennung (z. B. Windows-VM auf Linux-Host möglich).
  * **Container (Docker / Kubernetes):** Alle Container auf einem Host **teilen sich denselben Betriebssystem-Kernel des Hosts** und beinhalten nur die Anwendung samt ihren Bibliotheken $\rightarrow$ extrem klein (wenige MB), startet in Millisekunden, spart massiv RAM und CPU!

---

### 4. Cloud Computing (AP1 Frühjahr 2022)
* **Servicemodelle:**
  * **IaaS (*Infrastructure as a Service*):** Miete virtueller IT-Infrastruktur (VMs, Storage, Netzwerke). Kunde installiert und wartet ab dem Betriebssystem alles selbst *(z. B. AWS EC2, Hetzner Cloud, Azure VMs)*.
  * **PaaS (*Platform as a Service*):** Anbieter betreibt Hardware + OS + Datenbank/Laufzeitumgebung; Entwickler lädt nur noch seinen Anwendungscode hoch *(z. B. Azure App Service, Google App Engine)*.
  * **SaaS (*Software as a Service*):** Fertige Anwendungssoftware über Browser/App inkl. Wartung und Backups durch den Anbieter *(z. B. Microsoft 365, Google Workspace, Salesforce)*.
* **Bereitstellungsmodelle:**
  * **Public Cloud:** Infrastruktur wird von einem Cloud-Provider über das Internet für viele Kunden (*Multi-Tenancy*) bereitgestellt.
  * **Private Cloud:** Exklusiv für ein einziges Unternehmen betrieben (im eigenen Rechenzentrum *On-Premises* oder dediziert gehostet).
  * **Hybrid Cloud:** Kombination aus Private Cloud (für hochsensible Kerndaten nach DSGVO) und Public Cloud (z. B. zum Abfangen von Lastspitzen / *Cloud Bursting*).
