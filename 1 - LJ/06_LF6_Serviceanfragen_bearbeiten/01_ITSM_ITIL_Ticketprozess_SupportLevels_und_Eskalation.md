# 🎫 LF 6 – Teil 1: IT-Service-Management (ITIL), Ticket-Prozess, Support-Levels & Eskalation

> **Lernfeld 6 (Prüfungsrelevant in der BaWü-AP1!):** Serviceanfragen bearbeiten  
> **Video-Quelle (Go):** [▶️ Themenpool von LF5 und LF6](https://www.youtube.com/watch?v=KHf9IbBK8Z8)

---

## ✅ Checkliste der Lehrinhalte (LF 6 – Teil 1)
- [ ] Grundbegriffe nach **ITIL (IT Infrastructure Library):** *Incident* vs. *Problem* vs. *Service Request* vs. *Change* unterscheiden
- [ ] Den **Lebenszyklus eines Support-Tickets** von der Annahme bis zum Abschluss beschreiben
- [ ] **Priorisierung von Serviceanfragen** anhand der Matrix aus **Dringlichkeit $\times$ Auswirkung (Impact)** durchführen
- [ ] Aufgaben von **1st-Level-, 2nd-Level- und 3rd-Level-Support** abgrenzen
- [ ] **Funktionale (fachliche) vs. Hierarchische Eskalation** erklären

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Zentrale ITIL-Begriffe im IT-Service-Management (ITSM)

Verwechsle in der Prüfung niemals **Incident (Störung)** und **Problem**!

| ITIL-Begriff | Definition | Ziel des Prozesses | Typisches Beispiel |
| :--- | :--- | :--- | :--- |
| **Incident** *(Störung / Vorfall)* | Eine ungeplante Unterbrechung eines IT-Services oder eine Minderung der Qualität eines IT-Services. | **Schnellstmögliche Wiederherstellung des normalen Servicebetriebs** für den Kunden (ggf. auch durch eine vorübergehende Behelfslösung = **Workaround**!). | *„Mein PC kann nicht mehr auf dem Abteilungsdrucker drucken.“* ($\rightarrow$ Workaround: Vorübergehend auf den Flurdrucker umleiten). |
| **Problem** | Die **zugrundeliegende, oft noch unbekannte Ursache** für einen oder mehrere wiederkehrende Incidents. | **Ursachenforschung (*Root Cause Analysis*)** und dauerhafte Beseitigung der Fehlerquelle, damit die Störung künftig gar nicht mehr auftritt. *(Bekannte Fehler + Workarounds werden in der **KEDB – Known Error Database** dokumentiert).* | *„Jeden Morgen um 08:00 Uhr stürzt der Druckerspooler-Dienst auf dem Server wegen eines Speicherlecks im neuen Treiber ab.“* |
| **Service Request** *(Serviceanfrage)* | Eine Anfrage eines Benutzers nach Information, Beratung, einer Standardänderung oder dem Zugriff auf einen Service (**kein Fehler/Ausfall!**). | Schnelle, standardisierte Bereitstellung über definierte Workflows. | *„Bitte richten Sie für unsere neue Mitarbeiterin Frau Schulz ein E-Mail-Postfach und einen Zugang zum Ordner ‚Vertrieb‘ ein.“* |
| **Change** *(Änderung)* | Hinzufügen, Modifizieren oder Entfernen von IT-Komponenten, was sich auf IT-Services auswirken kann. | Kontrollierte Durchführung von Änderungen mit minimalem Ausfallrisiko (*Change Management*). | *„Upgrade des Datenbankservers von Version 14 auf Version 16 am kommenden Samstag.“* |

---

### 2. Der strukturierte Ablauf einer Serviceanfrage (Ticket-Prozess in 7 Schritten)

```text
1. Annahme &Identifikation (SPOC / User verifizieren)
       ↓
2. Ticket-Erfassung (Logging: Wer, Was, Seit wann, Fehlermeldung?)
       ↓
3. Kategorisierung (Hardware / Software / Netzwerk) & Priorisierung (Dringlichkeit x Impact)
       ↓
4. Erstdiagnose & Lösungsversuch im 1st-Level-Support (mittels FAQ / Known Error Database)
       ↓  (Falls im 1st-Level innerhalb der Zeitvorgabe nicht lösbar -> ESKALATION!)
5. Bearbeitung & Lösung im 2nd- / 3rd-Level-Support
       ↓
6. Lösungsdokumentation im Ticket & Rückmeldung an den Kunden (Prüfung durch Kunden!)
       ↓
7. Ticket-Abschluss (Closure) & Kundenzufriedenheitsabfrage + Wissensdatenbank aktualisieren
```

---

### 3. Wie wird die Priorität eines Tickets bestimmt? (Prioritätsmatrix!)

Die **Priorität** eines Tickets richtet sich **NICHT** danach, wie laut ein einzelner Anrufer am Telefon schimpft, sondern objektiv nach zwei Faktoren:
$$\mathbf{\text{Priorität} = \text{Auswirkung (Impact)} \times \text{Dringlichkeit (Urgency)}}$$

* **Auswirkung (*Impact*):** Wie viele Benutzer/Abteilungen sind betroffen und wie hoch ist der wirtschaftliche Schaden? (*Niedrig* = 1 Einzelplatz; *Mittel* = 1 Abteilung; *Hoch/Kritisch* = Gesamtes Unternehmen / Online-Shop steht still).
* **Dringlichkeit (*Urgency*):** Wie schnell wird der Dienst für kritische Geschäftsprozesse wieder benötigt bzw. gibt es einen zumutbaren Workaround?

| Auswirkung (Impact) \ Dringlichkeit | **Hoch** *(kein Workaround, sofortige Blockade)* | **Mittel** *(Workaround umständlich)* | **Niedrig** *(Zeitpunkt unkritisch)* |
| :--- | :---: | :---: | :---: |
| **Hoch** *(Ganze Firma / Kernsystem betroffen)* | 🔴 **Priorität 1 (Kritisch / Notfall)** | 🟠 **Priorität 2 (Hoch)** | 🟡 **Priorität 3 (Mittel)** |
| **Mittel** *(Eine Abteilung betroffen)* | 🟠 **Priorität 2 (Hoch)** | 🟡 **Priorität 3 (Mittel)** | 🟢 **Priorität 4 (Niedrig)** |
| **Niedrig** *(Einzelner Mitarbeiter betroffen)* | 🟡 **Priorität 3 (Mittel)** | 🟢 **Priorität 4 (Niedrig)** | ⚪ **Priorität 5 (Planbar)** |

---

### 4. Support-Levels (1st, 2nd, 3rd Level) & Arten der Eskalation

* **1st-Level-Support (*Service Desk / Helpdesk*):**
  * Bildet den **SPOC (*Single Point of Contact*)** – die einzige zentrale Anlaufstelle für alle Kunden/Mitarbeiter.
  * Nimmt Störungen per Telefon, E-Mail oder Self-Service-Portal auf, klassifiziert sie und löst direkt ca. $70\text{–}80\,\%$ aller Standardanfragen (*First-Call-Resolution-Rate*, z. B. Passwort zurücksetzen, Drucker einrichten, Standard-FAQ).
* **2nd-Level-Support:**
  * Erfahrene IT-Systemadministratoren und Techniker (auch Vor-Ort-Service / *Field Service*).
  * Übernimmt Tickets, die tieferes Fachwissen oder höhere Admin-Rechte erfordern (z. B. Firewall-Regeln, Server-Konfiguration, Datenbank-Analyse).
* **3rd-Level-Support:**
  * Höchste technische Instanz: Eigene **Softwareentwickler**, **Systemarchitekten** oder der **Support des externen Hard-/Software-Herstellers** (z. B. Microsoft, Cisco, SAP) für Quellcode-Bugs und Firmware-Fehler.

#### Die 2 Arten der Eskalation (Beliebte Prüfungsfrage in LF 6!):
1. **Funktionale (fachliche) Eskalation:**
   * Das Ticket wird an eine **fachlich höher qualifizierte Stelle** weitergeleitet, weil das Know-how oder die Zugriffsrechte im 1st-Level-Support nicht ausreichen (*1st-Level $\rightarrow$ 2nd-Level $\rightarrow$ 3rd-Level*).
2. **Hierarchische (disziplinarische) Eskalation:**
   * Information bzw. Einschaltung von **Vorgesetzten / Management** (z. B. IT-Leiter, Service-Level-Manager, Geschäftsführung), weil vereinbarte **SLA-Zeiten drohen gerissen zu werden**, dringend zusätzliche Ressourcen/Budgets für einen Notfall benötigt werden oder ein Konflikt mit einem wichtigen Kunden eskaliert!
