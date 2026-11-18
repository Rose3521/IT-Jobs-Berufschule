# 🛡️ LF 4 – Teil 1: Schutzziele (CIA-Triade), BSI IT-Grundschutz & Risikoanalyse

> **Lernfeld 4:** Schutzbedarfsanalyse im eigenen Arbeitsbereich durchführen  
> **AP1-Statistik:** *Schutzziele (15 P.), Schutzbedarf (6 P.), BSI & Risikoanalyse (5 P.)* = **26 Punkte** in der AP1!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Die 3 Grundschutzziele (CIA-Triade):** Vertraulichkeit, Integrität, Verfügbarkeit (+ Authentizität & Verbindlichkeit) erklären und Maßnahmen zuordnen
- [ ] **BSI IT-Grundschutz (200-1 bis 200-3)** & **ISO/IEC 27001** (ISMS) erläutern
- [ ] **Schutzbedarfskategorien (Normal, Hoch, Sehr hoch)** bestimmen und begründen
- [ ] **Maximumprinzip**, **Kumulationseffekt** und **Verteilungseffekt** unterscheiden
- [ ] **Risikoanalyse** durchführen und die **4 Strategien zur Risikobehandlung** anwenden

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Die Schutzziele der Informationssicherheit (CIA-Triade)

| Schutzziel | Englischer Begriff | Definition | Konkrete technische & organisatorische Maßnahmen |
| :--- | :--- | :--- | :--- |
| **Vertraulichkeit** | **C**onfidentiality | Daten dürfen **ausschließlich von autorisierten (befugten) Personen** gelesen oder eingesehen werden. | Verschlüsselung (**AES, BitLocker, TLS/HTTPS, VPN**), Zugriffsrechte (**ACL, Least Privilege**), Passwortschutz & **2FA/MFA**, Blickschutzfolie |
| **Integrität** | **I**ntegrity | Daten müssen **vollständig, korrekt und unverfälscht** sein. Unbefugte oder technische Manipulationen müssen verhindert bzw. erkannt werden. | **Kryptografische Hashwerte / Prüfsummen (SHA-256)**, **Digitale Signatur**, Schreibschutz, Datenbank-Transaktionen (ACID), **ECC-RAM** |
| **Verfügbarkeit** | **A**vailability | Daten, Dienste und IT-Systeme müssen für Berechtigte **innerhalb einer vereinbarten Zeitspanne zuverlässig nutzbar** sein. | **RAID (1, 5, 6, 10)**, **Backup-Konzept**, **USV & Notstromaggregat**, Redundante Server/Netzteile/Internetleitungen, Loadbalancer, DDoS-Schutz |
| **Authentizität** | Authenticity | Echtheit und überprüfbare Identität eines Kommunikationspartners oder einer Datenquelle. | Digitale Zertifikate (**PKI / X.509**), Digitale Signatur, 2FA |
| **Verbindlichkeit** | Non-Repudiation | **Nichtabstreitbarkeit:** Eine durchgeführte Aktion oder gesendete Nachricht kann im Nachhinein nicht geleugnet werden. | Qualifizierte elektronische Signatur, revisionssichere Audit-Logs |

---

### 2. Schutzbedarfsfeststellung nach BSI IT-Grundschutz (BSI-Standard 200-2)

Der Schutzbedarf wird zunächst für die **Geschäftsprozesse und Daten/Anwendungen** festgelegt und vererbt sich dann auf die darunterliegende **IT-Infrastruktur (Server, Netzwerke, Räume)**:

| Schutzbedarfskategorie | Beschreibung der möglichen Schadensfolgen | Typische Beispiele |
| :--- | :--- | :--- |
| **Normal** | Schadensauswirkungen sind **begrenzt und überschaubar** (geringer finanzieller Verlust, ein Ausfall von 24–48 Stunden ist ohne größere Probleme verkraftbar). | Einzelplatz-PC ohne lokale Daten, öffentlicher Webauftritt ohne Shop, Testsystem |
| **Hoch** | Schadensauswirkungen können **beträchtlich** sein (hoher finanzieller Schaden, spürbarer Ansehens-/Imageverlust, Verstoß gegen Gesetze/DSGVO). | Zentraler E-Mail-, Datei- oder ERP-Server, Personaldatenbank (HR), Kundendatenbank |
| **Sehr hoch** | Schadensauswirkungen können **existenzbedrohend / katastrophal** sein oder eine **Gefahr für Leib und Leben** von Menschen darstellen. | Intensivstation im Krankenhaus, Flugverkehrskontrolle, Kernsystem einer Bank, geheime Patentdaten |

* **Vererbungsregeln bei der Schutzbedarfsfeststellung:**
  * **Maximumprinzip:** Laufen auf einem Virtualisierungsserver drei virtuelle Maschinen mit den Schutzbedarfen *Normal*, *Normal* und *Hoch*, so erbt der physische Server automatisch den **höchsten Schutzbedarf (*Hoch*)**!
  * **Kumulationseffekt:** Werden sehr viele Daten oder Systeme mit jeweils nur *normalem* Schutzbedarf an einer zentralen Stelle gebündelt (z. B. ein zentraler Core-Switch für 500 Arbeitsplätze), steigt der Schutzbedarf dieser Komponente durch die Summenwirkung auf **Hoch**!
  * **Verteilungseffekt:** Wird ein Dienst mit *sehr hohem* Schutzbedarf (Verfügbarkeit) auf einen redundanten Server-Cluster aus 4 identischen Knoten verteilt, kann der Verfügbarkeits-Schutzbedarf für den *einzelnen* Serverknoten auf *Normal* oder *Hoch* sinken.

---

### 3. Risikoanalyse & Die 4 Strategien der Risikobehandlung (BSI 200-3)
$$\text{Risiko} = \text{Eintrittswahrscheinlichkeit} \times \text{Schadenshöhe}$$

1. **Risiko vermeiden (*Avoid*):** Die risikobehaftete Aktivität oder Technologie komplett einstellen/abschalten *(z. B. unsicheren FTP-Server komplett abschalten)*.
2. **Risiko vermindern / reduzieren (*Mitigate / Reduce*):** Durch technische oder organisatorische Sicherheitsmaßnahmen die Eintrittswahrscheinlichkeit oder die Schadenshöhe senken *(z. B. Firewall, MFA und tägliches Backup einrichten)*.
3. **Risiko überwälzen / transferieren (*Transfer / Share*):** Die finanziellen Folgen an einen Dritten auslagern *(z. B. Abschluss einer **Cyber-Versicherung** oder Auslagerung in ein zertifiziertes Rechenzentrum mit SLA)*.
4. **Risiko akzeptieren (*Accept*):** Das verbleibende **Restrisiko** bewusst durch die Geschäftsleitung tragen, wenn die Kosten weiterer Schutzmaßnahmen höher wären als der mögliche Schaden.
