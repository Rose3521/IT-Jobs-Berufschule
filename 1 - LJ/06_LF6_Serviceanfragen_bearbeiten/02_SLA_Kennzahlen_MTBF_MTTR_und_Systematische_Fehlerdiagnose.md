# 🛠️ LF 6 – Teil 2: SLA-Kennzahlen (MTBF, MTTR), Verträge (SLA/OLA/UC) & Systematische Fehlerdiagnose

> **Lernfeld 6 (Prüfungsrelevant in der BaWü-AP1!):** Serviceanfragen bearbeiten

---

## ✅ Checkliste der Lehrinhalte (LF 6 – Teil 2)
- [ ] **SLA (*Service Level Agreement*)**, **OLA (*Operational Level Agreement*)** und **UC (*Underpinning Contract*)** abgrenzen
- [ ] **Reaktionszeit** vs. **Lösungs-/Wiederherstellungszeit** unterscheiden
- [ ] Zuverlässigkeitskennzahlen **MTBF**, **MTTR** und **MTTF** erklären und berechnen
- [ ] **Methoden zur systematischen Fehlersuche** (OSI Bottom-Up/Top-Down/Divide & Conquer, **5-Why-Methode**, **Ishikawa-Diagramm**, **PDCA-Zyklus**) anwenden
- [ ] **Datenschutz- und Sicherheitsregeln bei der Fernwartung** (TeamViewer, RDP, SSH) beachten

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Vertragsarchitektur im IT-Service: SLA vs. OLA vs. UC

Damit ein IT-Dienstleister die im **SLA** gegenüber dem Kunden versprochenen Zeiten (z. B. *„Wiederherstellung innerhalb von 4 Stunden“*) überhaupt einhalten kann, braucht er im Hintergrund abgesicherte Verträge mit seinen eigenen Abteilungen und Zulieferern:

```text
+-------------------------------------------------------------------+
|                        KUNDE / AUFTRAGGEBER                       |
+-------------------------------------------------------------------+
                                 ↕  SLA (Service Level Agreement - Außenverhältnis!)
+-------------------------------------------------------------------+
|                  IT-DIENSTLEISTER (1st-Level Service Desk)        |
+-------------------------------------------------------------------+
             ↕  OLA (Intern!)                            ↕  UC (Externer Zulieferer!)
+-------------------------------+           +-----------------------------------+
| Interne Fachabteilung         |           | Externer Hardware-Hersteller / ISP|
| (z. B. eigenes Netzwerk-Team) |           | (z. B. Telekom, Dell, AWS)        |
+-------------------------------+           +-----------------------------------+
```

* **SLA (*Service Level Agreement*):** Vertrag zwischen **IT-Dienstleister und externem Kunden** über Art, Umfang und Qualität (Kennzahlen, Reaktionszeiten, Verfügbarkeit, Preis, Vertragsstrafen/Pönalen) eines IT-Services.
* **OLA (*Operational Level Agreement*):** **Interne** Vereinbarung zwischen verschiedenen **Abteilungen innerhalb desselben IT-Dienstleisters** (z. B. zwischen 1st-Level-Helpdesk und internem 2nd-Level-Datenbankteam: *„Das DB-Team übernimmt kritische Tickets innerhalb von 30 Minuten“*).
* **UC (*Underpinning Contract*):** Rechtsgültiger Vertrag des IT-Dienstleisters mit einem **externen Drittanbieter / Subunternehmer / Lieferanten** (z. B. Vertrag mit dem Glasfaser-Provider oder Server-Hersteller über einen 2-Stunden-Vor-Ort-Ersatzteilservice).

---

### 2. Zuverlässigkeits- & Service-Kennzahlen: MTBF, MTTR, MTTF & Verfügbarkeit

```text
Betrieb (System läuft)          Ausfall!        Wiederhergestellt!      Betrieb (System läuft)
|------------------------------|================|----------------------|-----------------------> Zeit
|<----------- MTTF ----------->|<---- MTTR ---->|<------- MTTF ------->|
|<------------------- MTBF -------------------->|
```

* **MTTF (*Mean Time To Failure* – Mittlere Betriebsdauer bis zum Ausfall):**
  * Durchschnittliche Zeitspanne, die ein System **am Stück fehlerfrei läuft**, bevor der nächste Fehler auftritt (reine *Uptime* zwischen zwei Ausfällen; wird auch für nicht-reparierbare Bauteile genutzt).
* **MTTR (*Mean Time To Repair / Recover* – Mittlere Reparatur-/Wiederherstellungsdauer):**
  * Durchschnittliche Zeitspanne vom **Eintritt des Ausfalls bis das System wieder vollständig produktiv läuft** (*Downtime* pro Ausfall).
* **MTBF (*Mean Time Between Failures* – Mittlere Zeit zwischen zwei Ausfällen bei reparierbaren Systemen):**
  * Zeitspanne vom **Beginn eines Ausfalls bis zum Beginn des nächsten Ausfalls**:
    $$\mathbf{\text{MTBF} = \text{MTTF} + \text{MTTR}}$$
* **Technische Verfügbarkeit $V$ (in %) aus MTTF und MTTR berechnen:**
  $$\mathbf{V = \frac{\text{MTTF}}{\text{MTTF} + \text{MTTR}} \times 100\,\% = \frac{\text{MTTF}}{\text{MTBF}} \times 100\,\%}$$

---

### 3. Methoden zur systematischen Fehleranalyse & Problemlösung (LF 6)

#### A. Fehlersuch-Strategien im Netzwerk (anhand des ISO/OSI-Modells)
1. **Bottom-Up-Ansatz (Von Schicht 1 nach Schicht 7):** Startet bei der physischen Verkabelung (Steckt das Kabel? Leuchtet die Link-LED am Switch?) $\rightarrow$ Layer 2 (MAC/VLAN) $\rightarrow$ Layer 3 (`ipconfig`, `ping` Gateway) $\rightarrow$ Layer 7 (DNS/Browser). *(Ideal, wenn ein einzelner Arbeitsplatz gar keine Verbindung mehr hat!).*
2. **Top-Down-Ansatz (Von Schicht 7 nach Schicht 1):** Startet bei der Anwendung (z. B. *„Nur das E-Mail-Programm geht nicht, aber Surfen im Browser klappt!“* $\rightarrow$ Da Surfen klappt, funktionieren Layer 1 bis 4 bereits einwandfrei; der Fehler muss auf Layer 7 in den SMTP/IMAP-Einstellungen liegen!).
3. **Divide-and-Conquer (*Teile und Herrsche / Halbierungsverfahren*):** Man testet in der **Mitte** auf **Schicht 3 (`ping` auf IP-Adresse)**:
   * Klappt der `ping` auf die IP $\rightarrow$ Layer 1, 2 und 3 sind in Ordnung (weiter nach oben Richtung Layer 4–7 suchen!).
   * Schlägt der `ping` fehl $\rightarrow$ Fehler liegt unten auf Layer 1, 2 oder 3!

#### B. Qualitätsmanagement- & Ursachenanalyse-Methoden
* **5-Why-Methode (*5-Warum-Fragetechnik*):** Man fragt bei einem Fehler bis zu fünfmal hintereinander **„Warum ist das passiert?“**, um nicht nur das oberflächliche Symptom zu bekämpfen, sondern die **wahre Grundursache (*Root Cause*)** zu finden.
  * *Beispiel:* 1. Warum steht der Server? $\rightarrow$ Sicherung ist rausgesprungen. 2. Warum? $\rightarrow$ USV war überlastet. 3. Warum? $\rightarrow$ Die Putzkolonne hat den Industriestaubsauger an die rote USV-Steckdose angeschlossen. 4. Warum? $\rightarrow$ Die USV-Steckdosen waren nicht beschriftet und hatten keine Spezialkodierung. *Wahre Lösung:* USV-Steckdosen mechanisch kodieren/abschließen!
* **Ishikawa-Diagramm (*Ursache-Wirkungs-Diagramm / Fischgrätendiagramm*):**
  * Visualisiert alle möglichen Ursachen eines Problems strukturiert entlang der **5M / 6M-Hauptgräten**: **Mensch, Maschine (Technik/Hardware), Methode (Prozess/Software), Material, Mitwelt (Umgebung/Klima), Messung**.
* **PDCA-Zyklus (*Demingkreis* zur kontinuierlichen Verbesserung – KVP):**
  1. **P – Plan (Planen):** Problem analysieren, Ziel und Maßnahmen festlegen.
  2. **D – Do (Umsetzen):** Maßnahmen im Kleinen / Testbetrieb ausprobieren.
  3. **C – Check (Überprüfen):** Soll-Ist-Vergleich durchführen (Hat die Maßnahme das Problem gelöst?).
  4. **A – Act (Standardisieren / Nachbessern):** Bei Erfolg als neuen Standard im Betrieb festschreiben.

---

### 4. Sicherheit & Datenschutz bei der Fernwartung (*Remote Support*)
Wenn sich ein Support-Mitarbeiter per Fernwartung (z. B. TeamViewer, AnyDesk, RDP, Quick Assist) auf den Bildschirm eines Kunden oder Kollegen schaltet, müssen zwingend folgende Regeln eingehalten werden:
1. **Explizite Einwilligung des Benutzers:** Der Zugriff darf niemals heimlich im Hintergrund erfolgen – der Benutzer muss die Sitzung aktiv bestätigen!
2. **Schutz vertraulicher Daten:** Der Benutzer muss vor Beginn aufgefordert werden, **offene vertrauliche Dokumente, E-Mails oder Personaldaten zu schließen**.
3. **Zuschauen & Abbruchmöglichkeit:** Der Benutzer muss am Bildschirm jederzeit sehen können, was der Techniker tut, und die Verbindung jederzeit per Knopfdruck sofort trennen können.
4. **Verschlüsselte Verbindung & Protokollierung:** Die Übertragung muss Ende-zu-Ende-verschlüsselt sein, und die Sitzung (Zeitpunkt, Techniker, Dauer, Grund) wird im Ticket dokumentiert (**keine Videoaufzeichnung ohne ausdrückliche Genehmigung!**).
