# 💬 BWL & PM – Teil 5: Nutzwertanalyse, Leasing vs. Kauf, SLA, Kundenberatung & 4-Ohren-Modell

> **AP1-Statistik:** *Nutzwertanalyse (45 P. – Platz 1 aller Einzelthemen!), Leasing (19 P.), Englisch (16 P.), Kundenberatung/Präsentieren/Schulung/4-Ohren-Modell (32 P.)* = **112 Punkte** in der AP1!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Nutzwertanalyse (NWA)** vollständig berechnen, begründen und kritisch bewerten
- [ ] **Finanzierungsformen (Kauf vs. Leasing vs. Miete)** aus betriebswirtschaftlicher und steuerlicher Sicht vergleichen
- [ ] **IT-Support-Levels (1st, 2nd, 3rd Level)** & **Service Level Agreement (SLA: Reaktionszeit vs. Wiederherstellungszeit)** erklären
- [ ] **4-Ohren-Modell (Schulz von Thun)** auf Kundengespräche anwenden
- [ ] Regeln für **Präsentationen, Kundenschulungen, E-Mails** und **englische Fachtexte** beherrschen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Nutzwertanalyse (NWA) – Platz 1 aller Einzelthemen in der AP1 (45 Punkte!)
* **Zweck:** Entscheidungsmethode beim Angebotsvergleich, um neben dem reinen Preis auch **qualitative (nicht direkt in Euro messbare) Kriterien** wie *Supportqualität, Bedienbarkeit, Erweiterbarkeit oder Nachhaltigkeit* systematisch zu bewerten.
* **Die 5 Schritte bei der Erstellung in der Prüfung:**
  1. **K.O.-Kriterien (Muss-Kriterien) vorab prüfen:** Erfüllt ein Angebot eine zwingende Mindestanforderung des Kunden nicht (z. B. *„Mindestens 32 GB RAM“* oder *„Lieferung binnen 7 Tagen“*), scheidet es **sofort vor der Berechnung** aus!
  2. **Bewertungskriterien festlegen und gewichten:** Die Summe aller Gewichtungen muss zwingend **$100\,\%$ (bzw. $1{,}0$)** ergeben.
  3. **Punkte vergeben:** Jedem Anbieter pro Kriterium Punkte auf einer einheitlichen Skala (z. B. `1` = ungenügend bis `10` = sehr gut) zuweisen.
  4. **Teilnutzwerte und Gesamtnutzwert berechnen:**
     $$\text{Teilnutzwert} = \text{Gewichtung} \times \text{Punkte} \qquad \longrightarrow \qquad \text{Gesamtnutzwert} = \sum \text{Teilnutzwerte}$$
  5. **Entscheidungssatz formulieren & NWA kritisch hinterfragen:**
     * *Vorteil der NWA:* Macht komplexe Entscheidungen mit weichen Faktoren transparent und nachvollziehbar.
     * *Nachteil / Kritik an der NWA:* Die Auswahl der Kriterien, die Prozent-Gewichtung und die Punktevergabe sind **subjektiv** und können das Ergebnis verzerren.

---

### 2. Finanzierungsvergleich: Kauf vs. Leasing vs. Miete (AP1: 19 Punkte!)

| Kriterium | Kauf (Eigen- oder Kreditfinanzierung) | Leasing (Finanzierungs-/Operate-Leasing) | Miete (§ 535 BGB) |
| :--- | :--- | :--- | :--- |
| **Eigentümer der Hardware** | **Käufer** (nach vollständiger Zahlung). | **Leasinggesellschaft (Leasinggeber)** – Kunde ist nur Besitzer/Nutzer. | **Vermieter**. |
| **Liquidität & Kapitalbindung** | **Hohe einmalige Kapitalbindung** (schwächt die Liquidität bei Barzahlung). | **Schont das Eigenkapital und die Liquidität** (*Pay as you earn* – gleichmäßige monatliche Raten). | Schont die Liquidität (monatliche Miete). |
| **Steuerliche Auswirkung** | Anschaffungskosten werden im Anlagevermögen aktiviert und über die Nutzungsdauer **abgeschrieben (AfA)** *(bei Computerhardware/Software seit 2021 auch 1-jährige Sofortabschreibung möglich)*. | Leasingraten sind sofort in voller Höhe als **laufende Betriebsausgaben steuerlich absetzbar** und belasten die Bilanz nicht (schont die Eigenkapitalquote / Kreditwürdigkeit!). | Mietzahlungen sind sofort als **Betriebsausgaben** absetzbar. |
| **Wartungs- & Ausfallrisiko** | Trägt der **Käufer** (nach Ablauf von Gewährleistung/Garantie). | **Achtung Prüfungsfalle:** Beim typischen Leasing wird das **Instandhaltungs-, Versicherungs- und Sachgefahr-Risiko vertraglich auf den Leasingnehmer (Kunden) abgewälzt**! | Gesetzlich muss der **Vermieter** das Gerät auf eigene Kosten instand halten und bei Defekt ersetzen! |
| **Planung & Modernität** | Langfristig am günstigsten, wenn die Hardware viele Jahre genutzt wird. | Ideal bei kurzen Innovationszyklen: Nach 36 Monaten gibt man die alten Laptops einfach zurück und least die neueste Generation (*dafür während der festen Grundmietzeit **nicht kündbar** und in der Gesamtsumme teurer als Kauf!*). | Sehr flexibel für kurzfristigen Projektbedarf, auf Dauer aber am teuersten. |

---

### 3. IT-Support-Organisation & Service Level Agreement (SLA)
* **Support-Stufen:**
  * **1st-Level-Support (Service Desk / Helpdesk):** Erster Ansprechpartner (**SPOC – *Single Point of Contact***). Erfasst das Ticket, vergibt eine **Priorität (aus Dringlichkeit $\times$ Auswirkung/Impact)** und löst bekannte Standardprobleme (Passwort-Reset, Berechtigungen) per Wissensdatenbank.
  * **2nd-Level-Support:** Spezialisierte Systemadministratoren für tiefere technische Probleme an Servern, Netzwerken oder Datenbanken.
  * **3rd-Level-Support:** Eigene Softwareentwickler, Systemarchitekten oder der externe Hersteller-Support für Programmierfehler (Bugs) und Hardwaredefekte.
* **Wichtige Kennzahlen im SLA (*Service Level Agreement*):**
  * **Servicezeiten:** Zeitraum, in dem der Support erreichbar ist (z. B. *Mo–Fr 08:00–18:00 Uhr* vs. *$24/7$*).
  * **Reaktionszeit (*Response Time*):** Maximale Zeit von der Ticket-Meldung bis ein Techniker **mit der Bearbeitung beginnt** (nicht bis zur Lösung!).
  * **Wiederherstellungszeit (*Resolution Time / MTTR*):** Maximale Zeit von der Störungsmeldung bis das System **wieder fehlerfrei läuft**.
  * **Verfügbarkeit in %:** z. B. $99{,}9\,\%$ im Jahr = maximal **$8{,}76\text{ Stunden}$ erlaubter Ausfall** im gesamten Jahr (bei Überschreitung droht eine **Vertragsstrafe / Pönale**!).

---

### 4. Kundenkommunikation: Das 4-Ohren-Modell (Schulz von Thun – AP1 Herbst 2023)

Jede gesprochene oder geschriebene Äußerung im Kundengespräch enthält gleichzeitig **4 Seiten (4 Schnäbel des Senders / 4 Ohren des Empfängers)**:

```text
                        +-----------------------------+
                        |       1. SACHINHALT         |
                        | (Daten, Fakten: Worüber?)   |
                        +-----------------------------+
                                       ^
                                       |
+-------------------------+    +---------------+    +-------------------------+
|   2. SELBSTKUNDGABE     | <--|   NACHRICHT   |--> |       4. APPELL         |
| (Ich-Botschaft: Gefühle,|    +---------------+    | (Aufforderung: Was soll |
|  Zustand des Senders)   |            |            |  der Empfänger tun?)    |
+-------------------------+            v            +-------------------------+
                        +-----------------------------+
                        |     3. BEZIEHUNGSEBENE      |
                        | (Du-/Wir-Botschaft: Wie     |
                        |  stehen wir zueinander?)    |
                        +-----------------------------+
```

* **Praxisbeispiel für die Prüfung:**  
  Ein Abteilungsleiter ruft beim IT-Support an und sagt gereizt: *„Schon wieder ist das Warenwirtschaftssystem abgestürzt!“*
  1. **Sachohr:** Das Warenwirtschaftssystem ist aktuell nicht erreichbar, und das kam in letzter Zeit bereits mehrfach vor.
  2. **Selbstkundgabe-Ohr:** Ich bin gestresst, genervt und stehe unter Druck, weil meine Abteilung nicht arbeiten kann.
  3. **Beziehungsohr:** Ihr von der IT-Abteilung habt eure Systeme nicht im Griff / lasst uns im Stich.
  4. **Appellohr:** Beheben Sie den Ausfall sofort und sorgen Sie dafür, dass das nicht wieder passiert!
  * **Wie reagiert der IT-Mitarbeiter professionell (*Deeskalation*)?** Er reagiert **nicht** beleidigt auf dem Beziehungsohr (*„Sie bedienen das Programm bestimmt wieder falsch!“*), sondern zeigt Verständnis für die Selbstkundgabe (*„Ich verstehe, dass das für Ihre Abteilung gerade sehr ärgerlich ist“*) und wechselt konstruktiv auf die **Sach- und Appellebene** (*„Ich prüfe sofort den Datenbankserver und rufe Sie in 10 Minuten mit einem Status-Update zurück.“*).
