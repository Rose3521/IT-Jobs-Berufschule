# 💻 LF 4 & BfK-I LS 10: Endpoint-Security, System-Härtung, GPO, Remotearbeit & Passwortsicherheit

> **Lernfeld 4 & BfK-I LS 10:** Server & Endgeräte absichern und von der Ferne sicher verwalten  
> **AP1-Statistik:** *Endpoint-Security (45 % Häufigkeit / 13 P.), Remotearbeit (12 P.), Passwortsicherheit (10 P.), Härtung, GPO & Hashverfahren (12 P.)* = **47 Punkte** in der AP1!

---

## ✅ Checkliste der Lehrinhalte (LS 10)
- [ ] **Endpoint-Security** & **System-Härtung (*OS/Server Hardening*)** erklären
- [ ] Zentrale Verwaltung über **Gruppenrichtlinien (GPO)** im Active Directory erläutern
- [ ] **Remotearbeit / Homeoffice / Mobiles Arbeiten** technisch und organisatorisch absichern
- [ ] **Passwortsicherheit** (BSI-Empfehlungen, MFA) & **Passwort-Hashing mit Salt (`bcrypt`, `Argon2`)** erklären

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Endpoint-Security & System-Härtung (*Hardening* – AP1: 45 % Häufigkeit!)

Unter **Endpoint-Security** versteht man den ganzheitlichen Schutz aller Endgeräte (Laptops, PCs, Smartphones, Server), die auf das Unternehmensnetzwerk zugreifen. Unter **System-Härtung (*Hardening*)** versteht man das gezielte **Verkleinern der Angriffsfläche (*Attack Surface*)** eines Betriebssystems oder Servers durch sichere Grundkonfiguration:

1. **Unnötige Dienste, Ports und Software abschalten/deinstallieren:** Jedes nicht benötigte Programm (*Bloatware*) und jeder offene Netzwerkport (z. B. Telnet, SMBv1, FTP) ist ein potenzielles Einfallstor und wird deaktiviert.
2. **Automatisches Patch-Management:** Betriebssystem, Firmware und Drittanbieter-Programme zeitnah über einen zentralen Update-Server (z. B. WSUS / Intune / Ansible) aktualisieren.
3. **Prinzip der minimalen Rechte (*Least Privilege*):** Normale Anwender arbeiten im Alltag **niemals mit lokalen Administratorrechten**! Für administrative Aufgaben gibt es getrennte Admin-Konten.
4. **EDR (*Endpoint Detection and Response*) & Antivirus + Personal Firewall:** Überwacht Prozesse in Echtzeit auf verdächtiges Verhalten (z. B. Massenverschlüsselung durch Ransomware) und isoliert befallene Clients sofort vom Netzwerk.
5. **Vollständige Laufwerksverschlüsselung (*Full Disk Encryption*):** Aktivierung von **BitLocker** (Windows, mit **TPM-Chip**), FileVault (macOS) oder LUKS (Linux), damit bei Diebstahl des Geräts niemand die SSD in einem anderen PC auslesen kann.
6. **Hardware- & Boot-Schutz:** **UEFI/BIOS-Passwort** vergeben, **Secure Boot** aktivieren, Booten von externen USB-Sticks deaktivieren.
7. **Standard-Zugangsdaten ändern & ungenutzte Standard-Accounts (`Gast`) deaktivieren**.

---

### 2. Gruppenrichtlinien (GPO – *Group Policy Objects*) im Active Directory (AP1 2023F)
In einer Windows-Domäne ermöglichen **Gruppenrichtlinien (GPOs)** dem Administrator, Sicherheits- und Konfigurationseinstellungen **zentral auf dem Domänencontroller** festzulegen und automatisch auf hunderte Clients und Benutzer auszurollen:
* **Typische GPO-Sicherheitsmaßnahmen in der Prüfung:**
  * **USB-Wechseldatenträger sperren** (verhindert das Einstecken fremder USB-Sticks $\rightarrow$ Schutz vor Malware und Datenklau),
  * **Automatische Bildschirmsperre** nach 5 Minuten Inaktivität erzwingen,
  * **Passwort-Richtlinien** (Mindestlänge, Kontosperrung nach 5 Fehlversuchen für 30 Minuten),
  * **Software-Restriktionsrichtlinien / AppLocker (Whitelisting):** Nur ausdrücklich vom Admin freigegebene `.exe`-Programme dürfen ausgeführt werden (blockiert ausführbare Anhänge aus dem `Downloads`-Ordner!),
  * Automatische Zuweisung von **Netzlaufwerken und Netzwerkdruckern** je nach Abteilung (Organisationseinheit / OU).

---

### 3. Remotearbeit / Homeoffice / Mobiles Arbeiten absichern (AP1 2021H, 2023H – 12 Punkte!)

| Technische Maßnahmen für Homeoffice & Dienstreisen | Organisatorische Maßnahmen (Richtlinien für Mitarbeiter) |
| :--- | :--- |
| **1. VPN-Zwang (*End-to-Site VPN*):** Zugriff auf Firmenserver nur über einen verschlüsselten VPN-Tunnel mit **2FA/MFA**. | **1. Clear-Screen- & Clean-Desk-Policy:** Bildschirm bei jedem Verlassen des Arbeitsplatzes sofort sperren (`Win + L`), vertrauliche Unterlagen wegschließen (auch gegenüber Familienmitgliedern/Besuchern!). |
| **2. Festplattenverschlüsselung (BitLocker):** Schützt Daten bei Verlust oder Diebstahl des Notebooks im Zug/Hotel. | **2. Verbot der Nutzung durch Dritte & kein BYOD:** Nur firmeneigene, zentral verwaltete Dienstgeräte nutzen; Kinder/Partner dürfen den Dienst-Laptop nicht benutzen. |
| **3. Blickschutzfolie (*Privacy Filter*):** Verdunkelt das Display ab einem seitlichen Winkel von $>30^\circ$, sodass Sitznachbarn im Zug/Flugzeug nichts mitlesen können (*Schutz vor Shoulder Surfing*). | **3. Vorsicht in der Öffentlichkeit:** Keine vertraulichen Kundengespräche im Großraumabteil des Zuges führen; fremde USB-Ladestationen meiden (*Juice Jacking*). |
| **4. MDM (*Mobile Device Management*):** Erlaubt das zentrale Verwalten, Orten und im Verlustfall das **Fernlöschen (*Remote Wipe*)** von Dienst-Laptops und Smartphones. | **4. Regelmäßige Security-Awareness-Schulungen** (Sensibilisierung für Phishing und Social Engineering). |

---

### 4. Passwortsicherheit & Sichere Passwortspeicherung (Hashing + Salt)
* **Wie sieht ein sicheres Passwort heute aus (BSI-Empfehlung)?**
  * **Länge vor Komplexität:** Mindestens **12 bis 16 Zeichen** (oder ein langer **Merksatz / Passphrase** wie `MeinHundFristGerne3GrüneÄpfel!`),
  * **Für jeden Dienst ein eigenes Passwort** (Einsatz eines **Passwort-Managers** wie KeePass / Bitwarden),
  * **Kombination mit 2FA / MFA** (Wissen = Passwort + Besitz = Authenticator-App / Hardware-Token FIDO2/YubiKey + Inhärenz = Fingerabdruck/FaceID).
* **Wie speichert eine Software Passwörter sicher in der Datenbank? (AP1 Frühjahr 2025):**
  * **Niemals im Klartext** und auch **nicht symmetrisch verschlüsselt** (da sonst jeder mit dem Schlüssel alle Passwörter entschlüsseln könnte)!
  * Passwörter werden mit einer **langsamen, spezialisierten Einweg-Hashfunktion (`Argon2`, `bcrypt`, `PBKDF2`)** inklusive eines **individuellen Zufallswerts („Salt“)** gehasht:
    $$\text{Gespeicherter Wert} = \text{bcrypt}(\text{Passwort} + \text{Zufälliger Salt pro Nutzer})$$
  * **Zweck des Salts:** Verhindert den Einsatz vorberechneter **Rainbow-Tables** und sorgt dafür, dass zwei Benutzer, die zufällig dasselbe Passwort gewählt haben, trotzdem völlig unterschiedliche Hashwerte in der Datenbank erhalten!
