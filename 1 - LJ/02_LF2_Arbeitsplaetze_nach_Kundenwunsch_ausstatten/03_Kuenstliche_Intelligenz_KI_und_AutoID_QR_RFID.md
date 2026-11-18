# 🤖 LF 2 – Künstliche Intelligenz (KI) & Auto-ID-Systeme (QR-Codes, RFID)

> **Lernfeld 2 & LF 5:** Moderne Technologien am IT-Arbeitsplatz  
> **AP1-Statistik:** *Künstliche Intelligenz* brachte in den AP1-Prüfungen (2023F, 2025F, 2025H) bereits **20 Punkte** (Platz 10 aller Einzelthemen!); *QR-Codes & RFID* brachten **10 Punkte**!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Künstliche Intelligenz (KI):** Lernverfahren (Supervised, Unsupervised, Reinforcement Learning) & Generative KI (LLMs) erklären
- [ ] **Chancen und Risiken von KI** im Unternehmen (Halluzinationen, DSGVO, Urheberrecht, Bias) beurteilen
- [ ] **QR-Codes:** Aufbau, Vorteile gegenüber 1D-Barcodes & Reed-Solomon-Fehlerkorrektur (L, M, Q, H) erläutern
- [ ] **RFID:** Funktionsweise, **aktive vs. passive Transponder** & Vorteile gegenüber optischen Codes nennen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Künstliche Intelligenz (KI / AI) in der AP1 (20 Punkte!)

#### A. Die 3 klassischen Lernverfahren des Machine Learnings (ML)
1. **Überwachtes Lernen (*Supervised Learning*):**
   * Das System wird mit **gelabelten Trainingsdaten** (Eingabedaten + bekannte korrekte Lösung) gefüttert, bis es Zusammenhänge erkennt und auf neue Daten anwenden kann.
   * *Beispiele:* Spam-Filter (trainiert mit Millionen als „Spam“/„Kein Spam“ markierten E-Mails), Bilderkennung (Katze vs. Hund), Kreditwürdigkeitsprognose.
2. **Unüberwachtes Lernen (*Unsupervised Learning*):**
   * Das System erhält **ungelabelte Rohdaten** ohne vorgegebene Lösung und sucht selbstständig nach versteckten Mustern, Strukturen oder Gruppen (**Clustering / Anomalieerkennung**).
   * *Beispiele:* Erkennung ungewöhnlicher Netzwerkzugriffe in einer Firewall (IDS), Kundengruppen-Segmentierung im Online-Shop.
3. **Bestärkendes Lernen (*Reinforcement Learning*):**
   * Ein Software-Agent lernt durch **Versuch und Irrtum (*Trial and Error*)** in einer Umgebung, indem er für richtige Aktionen eine **Belohnung (*Reward*)** und für Fehler eine **Bestrafung (*Penalty*)** erhält.
   * *Beispiele:* Schach-/Go-KI, autonome Robotersteuerung, Verkehrsflussoptimierung.

#### B. Generative KI & Large Language Models (LLMs) – Chancen vs. Risiken im IT-Betrieb

| Chancen / Einsatzgebiete im Unternehmen | Risiken & Gefahren (Sehr beliebte Prüfungsfrage!) |
| :--- | :--- |
| **1. IT-Support:** Chatbots beantworten Standardfragen rund um die Uhr ($24/7$), klassifizieren und fassen Support-Tickets automatisch zusammen. | **1. Halluzinationen (Fehlende Verlässlichkeit):** LLMs berechnen nur statistisch wahrscheinliche Wortfolgen – sie „wissen“ nichts und **erfinden plausibel klingende Falschaussagen, nicht existierende Gesetze oder fehlerhaften Code**! ($\rightarrow$ *Human-in-the-Loop*: Menschliche Kontrolle ist Pflicht!). |
| **2. Softwareentwicklung:** Code-Vervollständigung, Generierung von SQL-Abfragen, Erstellung von Unit-Tests, Erklärung fremden Codes, Dokumentation. | **2. Datenschutz (DSGVO) & Geheimnisverrat:** Werden echte Kundendaten, Patientenakten oder geheimer Firmen-Quellcode in öffentliche Cloud-KIs eingegeben, fließen diese auf fremde Server ab und werden evtl. zum Training genutzt! *(Lösung: Lokale On-Premises-KI oder Enterprise-Vertrag mit AVV).* |
| **3. Text- & Wissensarbeit:** Übersetzungen, Zusammenfassungen langer Protokolle, E-Mail-Entwürfe. | **3. Urheberrecht & Lizenzprobleme:** Gefahr, dass KI-generierter Code geschützte Open-Source-Lizenzen (wie GPL) verletzt; an rein KI-erzeugten Werken besteht kein eigenes Urheberrecht. |
| **4. IT-Security:** Schnelle Analyse riesiger Logdateien auf Angriffsmuster. | **4. Bias (Verzerrung / Diskriminierung)** durch einseitige Trainingsdaten sowie **Prompt Injection** (Manipulation der KI durch versteckte Befehle) und **hoher Stromverbrauch**. |

---

### 2. Auto-ID-Systeme: QR-Codes & RFID (AP1 2022H, 2025H – 10 Punkte!)

| Merkmal | 1D-Strichcode (Barcode, z. B. EAN-13) | 2D-Code: QR-Code (*Quick Response*) | RFID (*Radio Frequency Identification*) |
| :--- | :--- | :--- | :--- |
| **Abtastung** | Optisch (nur horizontal in 1 Dimension) | Optisch (zweidimensionale Punktmatrix mit **3 Positionsquadraten** in den Ecken $\rightarrow$ aus **$360^\circ$ jedem Winkel** lesbar!) | **Per Funkwellen (elektromagnetisches Feld)** zwischen Lesegerät (*Reader*) und Chip (*Transponder/Tag*). |
| **Sichtkontakt nötig?** | **Ja, zwingend** | **Ja, zwingend** | **NEIN!** Funktioniert auch durch Kartons, Holz, Kleidung oder Schmutz hindurch! |
| **Mehrfacherfassung (*Pulkerfassung*)?** | Nein (jeder Artikel muss einzeln gescannt werden) | Nein (einzeln scannen) | **JA (*Bulk Reading*)!** Eine komplette Palette mit 200 Kartons wird beim Durchfahren durch das Lagertor **in 1 Sekunde gleichzeitig** erfasst! |
| **Fehlertoleranz / Besonderheit** | Ist ein Strich zerkratzt, ist der Code unlesbar. | Integrierte **Reed-Solomon-Fehlerkorrektur** (4 Stufen: **L = 7 %, M = 15 %, Q = 25 %, H = bis zu 30 %** der Fläche können zerstört/überdeckt sein und der Code ist trotzdem noch komplett lesbar!). | **Passiver RFID-Tag:** Keine eigene Batterie (bezieht Strom durch Induktion aus dem Lesegerät; cent-günstig, hält ewig, kurze Reichweite).<br>**Aktiver RFID-Tag:** Eigene Batterie (sendet aktiv bis $>100\text{ m}$, z. B. LKW-Maut, Container-Tracking). |
