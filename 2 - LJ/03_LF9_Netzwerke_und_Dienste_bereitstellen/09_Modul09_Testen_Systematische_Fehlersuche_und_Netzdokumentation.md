# 🧪 LF 9 – Modul 9: Testen, systematische Fehleranalyse & Netzdokumentation

> **Lernsituation:** *Bäckerei Sonnkorn* (Bestell-Web-App nach Umstellung aus zwei Filialen nicht mehr erreichbar $\rightarrow$ systematisch finden, testen, dokumentieren & übergeben)  
> **EduSysTrainings-Status:** 🟡 Einheiten `09-01` bis `09-03` im Lehrerprojekt noch 🚧 *In Vorbereitung* – hier das prüfungsrelevante Kernwissen als Arbeitsgrundlage.

---

## ✅ Checkliste der Lehrinhalte (Modul 9)
- [ ] **Systematische Fehlersuche** (*Bottom-up, Top-down, Divide-and-Conquer*) anhand einer Hypothesenkette durchführen statt blind zu raten
- [ ] **Erreichbarkeits- und Funktionstests** reproduzierbar planen (**Testfall, Vorbedingung, Schritte, erwartetes Ergebnis, Ist-Ergebnis, Status**)
- [ ] **Positiv- und Negativtests** kombinieren (nicht nur prüfen, was *klappen* soll, sondern auch was *blockiert* sein muss – z. B. Gäste-WLAN $\not\rightarrow$ internes Netz!)
- [ ] Eine vollständige, wartbare **Netzdokumentation** (Netzplan, Adress-/VLAN-Liste, Kommunikationsmatrix, Konfigurationsstand) erstellen
- [ ] Eine Lösung strukturiert mit **Übergabeprotokoll** und Einweisung an den Kunden übergeben

---

## 📌 Kernwissen & Prüfungsgrundlagen

### 1. Systematische Fehlersuche (Divide-and-Conquer am Beispiel Filial-Ausfall)
* **Schritt 1 – Eingrenzen (Was geht noch, was nicht?):** Betrifft es *alle* Filialen oder nur *zwei*? Geht Internet allgemein, oder nur die Bestell-App? Funktioniert der Zugriff per IP, aber nicht per DNS-Name?
* **Schritt 2 – Schicht-für-Schicht-Prüfkette:**
  1. **L1/L2:** Link/WLAN/VPN-Tunnel aktiv? (`Get-NetAdapter`, VPN-Status)
  2. **L3:** Richtige IP, Subnetzmaske, Gateway & Route zur Zentrale? (`ipconfig /all`, `route print`, `ping <Gateway>`, `tracert <App-Server>`)
  3. **L4:** Ist der Zielport erreichbar oder blockiert eine Firewall nach der Umstellung? (`Test-NetConnection <Host> -Port 443`)
  4. **L7:** Löst DNS auf die neue IP auf (`nslookup`), passt das TLS-Zertifikat, antwortet HTTP mit `200` statt `403`/`502`/`503` (`curl.exe -i`)?
* **Schritt 3 – Eine Änderung pro Test:** Niemals fünf Einstellungen gleichzeitig ändern, sonst weiß man nicht, was den Fehler behoben hat!

---

### 2. Struktur eines professionellen Testprotokolls
| Test-ID | Beschreibung / Prüffall | Typ | Erwartetes Ergebnis | Ist-Ergebnis | Status |
| :---: | :--- | :---: | :--- | :--- | :---: |
| **T01** | Filiale 1 öffnet `https://bestellung.sonnkorn.de` | Positiv | HTTP `200`, gültiges TLS-Zertifikat, Login möglich | *(eintragen)* | ⬜ |
| **T02** | Gast im Filial-WLAN versucht Zugriff auf Bestell-Backend-Admin | **Negativ** | Verbindung wird durch Firewall/VLAN blockiert (Timeout/Refused) | *(eintragen)* | ⬜ |
| **T03** | Backup-Wiederherstellung der Bestelldatenbank im Testsystem | Restore | Daten konsistent lesbar innerhalb RTO | *(eintragen)* | ⬜ |

---

### 3. Bestandteile einer guten Netzdokumentation & Übergabe
1. **Aktueller Netzplan** (logisch & physisch, inkl. Netzgrenzen/VLANs/Standorten)
2. **IP-Adress-, VLAN- & Portbelegungsliste** (IPv4 + IPv6, Gateways, DNS-Einträge)
3. **Kommunikationsmatrix & Firewall-Regelwerk** (wer darf mit wem über welchen Port?)
4. **Test- und Abnahmeprotokoll** (Nachweis aller Positiv- und Negativtests)
5. **Betriebs- & Notfallhinweise** (Backup/Restore-Ablauf, Ansprechpartner, Wartungsfenster – **niemals Passwörter im Klartext** in offene Dokumente schreiben!)

---

### 🎯 Meilenstein (Modul 9)
* Abgeschlossener **Fehleranalyse- & Testbericht** sowie vollständige **Netzdokumentation & Übergabeprotokoll** als Abschluss von LF 9.
* *(Querverweis 1. LJ: [LF 6 – Teil 2 (Fehlerdiagnose)](../../1%20-%20LJ/06_LF6_Serviceanfragen_bearbeiten/02_SLA_Kennzahlen_MTBF_MTTR_und_Systematische_Fehlerdiagnose.md))*
