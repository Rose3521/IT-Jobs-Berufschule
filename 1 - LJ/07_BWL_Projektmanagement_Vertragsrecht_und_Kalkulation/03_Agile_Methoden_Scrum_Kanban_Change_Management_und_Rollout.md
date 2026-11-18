# 🔄 BWL & PM – Teil 3: Vorgehensmodelle (Wasserfall vs. Scrum & Kanban), Change Management & Rollout

> **AP1-Statistik:** *Change Management (9 P.), Vorgehensmodelle (4 P.), Rollout (2 P.)* = **15 Punkte** in der AP1 (und **47 Punkte** in der AP2 FIAE)!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Wasserfallmodell** vs. **Agile Vorgehensmodelle** nach Einsatzgebiet, Vor- und Nachteilen vergleichen
- [ ] **Scrum:** Die **3 Rollen**, **5 Events** und **3 Artefakte** erklären
- [ ] **Kanban:** Kanban-Board, Pull-Prinzip und **WIP-Limits** erläutern
- [ ] **Change Management** (Ablauf bei Änderungsanforderungen / Change Requests) beschreiben
- [ ] **Rollout-Strategien** (*Big Bang*, *Phasenweiser/Pilot-Rollout*, *Parallelbetrieb*) beurteilen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Klassisches Wasserfallmodell vs. Agile Softwareentwicklung

| Kriterium | Klassisches Wasserfallmodell | Agile Vorgehensmodelle (Scrum / Kanban) |
| :--- | :--- | :--- |
| **Ablaufprinzip** | **Streng sequenziell (Phase für Phase):** Jede Phase (*Analyse $\rightarrow$ Entwurf $\rightarrow$ Implementierung $\rightarrow$ Test $\rightarrow$ Betrieb*) muss erst komplett abgeschlossen sein, bevor die nächste beginnt. | **Iterativ & Inkrementell:** Entwicklung in kurzen Zyklen (**Sprints** von 1–4 Wochen), an deren Ende jedes Mal ein lauffähiges, vorzeigbares Teilprodukt (**Inkrement**) steht. |
| **Umgang mit Änderungswünschen** | **Sehr starr und teuer:** Änderungen mitten im Projekt erfordern den Rücksprung in frühere Phasen. | **Maximal flexibel:** Neue Kundenwünsche können nach jedem kurzen Sprint neu ins Backlog priorisiert werden. |
| **Wann ist welches Modell ideal?** | Wenn **alle Anforderungen zu 100 % von Anfang an feststehen** und sich nicht ändern (z. B. Hardware-Rollout von 100 PCs, gesetzlich exakt vorgeschriebene Schnittstelle). | Wenn die Anforderungen zu Beginn **noch unklar sind oder sich dynamisch weiterentwickeln** (z. B. innovative App-/Webentwicklung). |

---

### 2. Scrum – Das agile Rahmenwerk im Detail

* **Die 3 Rollen (*Accountabilities* im Scrum Team):**
  1. **Product Owner (PO):** Vertritt die Fachseite und die Kundeninteressen. Er verwaltet und **priorisiert das Product Backlog** nach geschäftlichem Nutzen (*Value*) und entscheidet über das **„WAS“**.
  2. **Scrum Master (SM):** Verantwortlich für das Verständnis und die Einhaltung der Scrum-Regeln (*Servant Leader / Coach*). Er moderiert Meetings und räumt Hindernisse (**Impediments**) für das Team aus dem Weg. *(Achtung: Er ist **kein** disziplinarischer Vorgesetzter!).*
  3. **Developers (Entwicklerteam):** Interdisziplinäres (*cross-funktionales*), selbstorganisiertes Team (ca. 3–9 Personen), das entscheidet, **„WIE“** die Aufgaben im Sprint technisch umgesetzt werden.
* **Die 3 Artefakte:**
  1. **Product Backlog:** Geordnete, dynamische Gesamtliste aller Anforderungen (**User Stories** nach dem Muster: *„Als <Rolle> möchte ich <Funktion>, um <Nutzen zu erreichen>“*), Bugs und Wünsche für das Produkt *(Commitment: Produktziel)*.
  2. **Sprint Backlog:** Die für den aktuellen Sprint ausgewählten Product-Backlog-Einträge plus der konkrete Umsetzungsplan der Entwickler *(Commitment: Sprint-Ziel)*.
  3. **Increment (Produktinkrement):** Das am Ende des Sprints fertige, getestete und nutzbare Produktpaket *(Commitment: **Definition of Done – DoD** = Qualitätscheckliste, wann eine Aufgabe wirklich zu 100 % fertig ist)*.
* **Die 5 Events (Ereignisse mit fester Maximaldauer / *Timebox*):**
  1. **Der Sprint:** Das Herzstück von Scrum (feste Dauer von **1 bis maximal 4 Wochen**).
  2. **Sprint Planning:** Zu Beginn des Sprints legen PO und Developers fest, *was* im kommenden Sprint geschafft werden kann und *wie* es umgesetzt wird.
  3. **Daily Scrum:** Tägliches kurzes **15-Minuten-Abstimmungsmeeting** der Developers zur Planung der nächsten 24 Stunden und zum Erkennen von Blockaden.
  4. **Sprint Review (Am Ende des Sprints – Fokus: PRODUKT / WAS):** Das Team stellt das fertige **Inkrement** dem Kunden und den Stakeholdern vor und sammelt Feedback.
  5. **Sprint Retrospective (Ganz am Ende des Sprints – Fokus: PROZESS & TEAM / WIE):** Interner Rückblick des Scrum Teams auf die eigene Zusammenarbeit, Kommunikation und Werkzeuge, um Verbesserungen für den nächsten Sprint zu beschließen.

---

### 3. Kanban (Flussorientiertes agiles Arbeiten)
* Ideal für **IT-Support, Systemadministration und kontinuierliche Wartung** (wo man nicht 3 Wochen auf den nächsten Sprint warten kann!).
* **3 Kernprinzipien:**
  1. **Visualisierung der Arbeit** auf einem **Kanban-Board** (Spalten z. B. `Backlog` $\rightarrow$ `To Do` $\rightarrow$ `In Progress` $\rightarrow$ `Review/Testing` $\rightarrow$ `Done`).
  2. **Pull-Prinzip:** Entwickler/Techniker bekommen Aufgaben nicht von oben aufgedrückt (*Push*), sondern **ziehen** sich selbst die nächste Aufgabe, sobald sie frei sind.
  3. **WIP-Limit (*Work in Progress Limit*):** Begrenzt strikt die maximale Anzahl an Tickets, die sich **gleichzeitig** in der Spalte `In Progress` befinden dürfen (z. B. max. 3 Tickets gleichzeitig). *Vorteil:* Verhindert Multitasking-Verzettelung, deckt Engpässe (*Bottlenecks*) sofort auf und verkürzt die Durchlaufzeit (*Lead Time*)!

---

### 4. Change Management & Rollout-Strategien (AP1 2025H, 2026F – 11 Punkte!)

* **Ablauf des Change Managements bei Projekt-Änderungswünschen (*Change Requests*):**
  1. **Change Request (CR) schriftlich aufnehmen** (Was genau soll geändert werden?).
  2. **Impact Analysis (Auswirkungsanalyse):** Prüfen, wie sich die Änderung auf **Zeit (kritischer Pfad!), Kosten und Qualität/Architektur** auswirkt.
  3. **Freigabeentscheidung (Change Control Board / Kunde):** Nachtragsangebot vorlegen und Änderung schriftlich genehmigen oder ablehnen lassen.
  4. **Implementierung & Dokumentation:** Anpassung von Pflichtenheft, Netzplan und Budget.
* **Die 3 Rollout-Strategien bei der Einführung neuer Software/Hardware:**
  * **Big Bang (Stichtagsumstellung):** Altsystem wird abgeschaltet und Neusystem für alle gleichzeitig aktiviert (*Schnell & günstig, aber **maximales Ausfallrisiko** bei Fehlern!*).
  * **Phasenweiser Rollout / Pilotbetrieb (*Phased Rollout*):** Zuerst Einführung in einer einzelnen **Pilot-Abteilung**, Fehler beheben, danach schrittweise Abteilung für Abteilung ausrollen (*Geringes Risiko, überschaubare Support-Last!*).
  * **Parallelbetrieb:** Alt- und Neusystem laufen für einige Wochen gleichzeitig produktiv (*Höchste Sicherheit, aber doppelte Kosten und doppelter Eingabeaufwand!*).
