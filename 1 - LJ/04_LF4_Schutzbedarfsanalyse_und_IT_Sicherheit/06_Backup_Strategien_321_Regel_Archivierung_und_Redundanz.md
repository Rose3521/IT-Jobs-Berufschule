# 💾 LF 4 & BfK-I LS 13 / LS 15: Datensicherung (Backup-Strategien, 3-2-1-Regel) & Redundanz

> **Lernfeld 4 & BfK-I LS 13 / LS 15:** Sichern und Wiederherstellen von Daten / Redundanz und Resilienz in der IT  
> **AP1-Statistik:** *Datensicherung (10 P.) & Redundanz (4 P.)* kamen in 36 % der AP1-Prüfungen vor!

---

## ✅ Checkliste der Lehrinhalte (LS 13 & LS 15)
- [ ] **Datensicherung (Backup) vs. Archivierung vs. RAID/Redundanz** sauber abgrenzen
- [ ] **Die 3-2-1-Backup-Regel** (inkl. Air-Gap / Immutable Backup gegen Ransomware) erklären
- [ ] **Vollbackup, Differenzielles Backup und Inkrementelles Backup** nach Speicherbedarf, Sicherungsdauer und **Wiederherstellungsaufwand (Restore)** vergleichen und berechnen
- [ ] **RPO (*Recovery Point Objective*)** vs. **RTO (*Recovery Time Objective*)** unterscheiden

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Wichtige Begriffsabgrenzung: Backup vs. Archivierung vs. RAID
* **Backup (Datensicherung):** Kurz- bis mittelfristige Kopie des aktuellen Datenbestands mit dem Ziel, die Daten nach einem Verlust (Hardwaredefekt, Ransomware, versehentliches Löschen) **schnellstmöglich wiederherzustellen (*Restore*)**. Der Originaldatenbestand bleibt dabei im Produktivsystem erhalten!
* **Archivierung:** **Langfristige, unveränderbare (revisionssichere) Aufbewahrung** von nicht mehr aktiv benötigten Geschäftsdaten (z. B. Rechnungen und Bilanzen für **10 Jahre** nach HGB/GoBD). Nach der Auslagerung ins Archiv werden die Daten oft aus dem Produktivsystem gelöscht, um dort Platz zu schaffen.
* **RAID / Redundanz (LS 15):** Mehrfache Auslegung von Hardware (Festplatten im RAID, redundante Netzteile, Cluster-Server, USV), damit der Betrieb beim Ausfall eines Bauteils **ohne Unterbrechung weiterläuft (Hochverfügbarkeit)**. **Ersetzt niemals ein Backup**, da ein Löschbefehl oder ein Verschlüsselungstrojaner sofort auf alle gespiegelten RAID-Platten gleichzeitig geschrieben wird!

---

### 2. Die 3-2-1-Backup-Regel (Erweitert zur 3-2-1-1-0-Regel)
* **3 Kopien der Daten:** 1 primärer Produktivdatenbestand + mindestens **2 Backup-Kopien**.
* **2 verschiedene Speichermedien:** z. B. lokales NAS/SAN **und** Magnetband (*LTO-Tape*) oder Objektspeicher/Cloud (schützt vor Serienfehlern eines Medientyps).
* **1 Kopie an einem externen Standort (*Offsite*):** z. B. im Zweigwerk oder Cloud-Rechenzentrum (schützt bei Brand, Hochwasser oder Einbruch im Hauptgebäude).
* **+ 1 Kopie *Offline / Air-Gapped / Immutable* (WORM – *Write Once, Read Many*):** Mindestens ein Backup muss physisch getrennt oder mit einem **unveränderbaren Schreibschutz (*Object Lock*)** versehen sein, damit **Ransomware** selbst mit gestohlenen Admin-Zugangsdaten das Backup nicht löschen oder verschlüsseln kann!
* **+ 0 Fehler bei der regelmäßigen Wiederherstellungs-Prüfung (*Restore-Test*)!** *(Ein Backup, dessen Wiederherstellung nie getestet wurde, ist wertlos!).*

---

### 3. Die 3 Backup-Arten im Detail (Beliebte Rechenaufgabe in der AP1!)

*(Technischer Hintergrund: Das Dateisystem setzt bei jeder neu erstellten oder geänderten Datei das sog. **Archivbit** auf `1` = „Datei wurde seit der letzten Sicherung verändert“).*

| Kriterium | 1. Vollbackup (*Full Backup*) | 2. Differenzielles Backup | 3. Inkrementelles Backup |
| :--- | :--- | :--- | :--- |
| **Was wird gesichert?** | **Immer ALLE ausgewählten Daten komplett** (unabhängig davon, ob sie sich geändert haben). | Alle Daten, die sich **seit dem letzten VOLLBACKUP** geändert haben. | Nur die Daten, die sich **seit dem jeweils ALLERLETZTEN Backup** (egal ob Voll- oder Inkrementell!) geändert haben. |
| **Was passiert mit dem Archivbit?** | Wird zurückgesetzt (`0`). | Wird **NICHT** zurückgesetzt! *(Dadurch wächst das differenzielle Backup von Tag zu Tag immer weiter an!).* | Wird zurückgesetzt (`0`). *(Dadurch enthält jedes Tagesbackup wirklich nur die Änderungen dieses einen Tages!).* |
| **Speicherplatz & Dauer beim Sichern** | **Am höchsten & am langsamsten** | **Mittel** (wächst von Mo bis Fr kumulativ an) | **Am geringsten & am schnellsten!** |
| **Wiederherstellungsaufwand (Restore!)** | **Am einfachsten & schnellsten:** Man braucht **nur 1 Medium** (das letzte Vollbackup). | **Schnell:** Man braucht **maximal 2 Medien**: Das **letzte Vollbackup** + das **allerletzte differenzielle Backup** vor dem Crash! | **Am aufwendigsten & riskantesten:** Man braucht das **letzte Vollbackup + ALLE inkrementellen Backups seitdem** in exakter Reihenfolge! *(Ist ein einziges Tages-Backup defekt, fehlt alles ab diesem Tag!).* |

#### Muster-Rechenbeispiel für eine Backup-Woche (Sonntag bis Donnerstag):
* **Ausgangsdaten am Sonntag:** $500\text{ GB}$ (Sonntag = Vollbackup).
* **Neue/geänderte Daten pro Tag:** Montag $+10\text{ GB}$, Dienstag $+15\text{ GB}$, Mittwoch $+5\text{ GB}$, Donnerstag $+20\text{ GB}$ (Freitagmorgen vor dem Backup fällt der Server aus!).

| Tag | Bei **Inkrementellem Backup** (Mo–Do) | Bei **Differenziellem Backup** (Mo–Do) |
| :--- | :--- | :--- |
| **Sonntag (Voll)** | **$500\text{ GB}$** | **$500\text{ GB}$** |
| **Montag** | $10\text{ GB}$ *(nur Mo)* | $10\text{ GB}$ *(Mo)* |
| **Dienstag** | $15\text{ GB}$ *(nur Di)* | $10 + 15 = \mathbf{25\text{ GB}}$ *(Mo + Di)* |
| **Mittwoch** | $5\text{ GB}$ *(nur Mi)* | $10 + 15 + 5 = \mathbf{30\text{ GB}}$ *(Mo + Di + Mi)* |
| **Donnerstand** | $20\text{ GB}$ *(nur Do)* | $10 + 15 + 5 + 20 = \mathbf{50\text{ GB}}$ *(Mo bis Do komplett!)* |
| **Gesamter Backup-Speicherbedarf (So–Do)** | $500 + 10 + 15 + 5 + 20 = \mathbf{550\text{ GB}}$ | $500 + 10 + 25 + 30 + 50 = \mathbf{615\text{ GB}}$ |
| **Welche Backups müssen am Freitag beim Restore eingespielt werden?** | **5 Backups:** **Sonntag (Voll) + Montag + Dienstag + Mittwoch + Donnerstag** ($550\text{ GB}$) | **Nur 2 Backups:** **Sonntag (Voll) + Donnerstag (Diff.)**! *(Mo, Di und Mi werden beim Restore NICHT gebraucht, da Donnerstag bereits alle Änderungen seit Sonntag enthält!)* |

---

### 4. RPO vs. RTO (Kennzahlen für Notfallmanagement / Business Continuity)
* **RPO (*Recovery Point Objective* – Maximaler Datenverlust in Zeit):** *„Wie alt dürfen die Daten nach der Wiederherstellung maximal sein / wie viele Stunden Arbeit dürfen im schlimmsten Fall verloren gehen?“* (Bestimmt, **wie oft** Backups gemacht werden müssen – z. B. RPO = 1 Stunde $\rightarrow$ stündliche Snapshots/Replikation nötig!).
* **RTO (*Recovery Time Objective* – Maximale Ausfalldauer bis zum Wiederanlauf):** *„Wie lange darf es nach einem Ausfall maximal dauern, bis das System wieder produktiv läuft?“* (Bestimmt, **wie schnell** die Restore-Technik bzw. das Failover-System sein muss!).
