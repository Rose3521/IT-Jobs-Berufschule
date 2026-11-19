# 📋 LF 9 – Modul 1: Anforderungen an Netzwerk & Dienste analysieren, 3-Schichten-Architektur & Kommunikationsmatrix

> **Lernsituation:** *Praxisverbund Lindental* (Online-Terminbuchung über 3 Standorte) & *Weber Präzisionstechnik GmbH* (Anforderungsanalyse)  
> **EduSysTrainings-Status:** Einheit `01-01` ✅ eingearbeitet · Einheiten `01-02` & `01-03` 🚧 noch in Vorbereitung (Kernmethodik aus K-ITSM `01-01` bereits integriert)

---

## ✅ Checkliste der Lehrinhalte (Modul 1)
- [ ] **Client und Server** als *Rollen* (nicht Geräte!) unterscheiden und den **Initiator** jeder Verbindung bestimmen
- [ ] Einen lokalen Webserver (`python -m http.server 8000 --bind 127.0.0.1`) starten und Ports/Zustände mit `netstat` beobachten
- [ ] Aufgaben den Schichten der **Drei-Schichten-Architektur** (*Frontend, Backend, Datenbank*) zuordnen und deren Sicherheitsregeln begründen
- [ ] **Externe Dienste & Web-APIs** (SMTP 587, SMS-Gateway, Fachsoftware-API, Cronjob/Scheduler, Webhooks) inkl. DSGVO-Check planen
- [ ] Eine vollständige **Kommunikationsmatrix** (7 Spalten inkl. Netzgrenze & Status) und ein **Kommunikationsdiagramm** erstellen
- [ ] Kundenäußerungen sauber in **Beobachtung/Problem**, **Ursachenvermutung**, **Lösungsidee** und **überprüfbare Anforderung** trennen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Client-Server-Prinzip: Rollen statt Geräte
* **Anfrage (*Request*) & Antwort (*Response*):** Der **Client** stellt die Anfrage, der **Server** bietet einen Dienst an, wartet passiv auf einem Port und antwortet.
* **Der Client ist immer der Initiator:** Er baut die Verbindung aktiv auf. **Warum entscheidend?** Firewalls filtern primär danach, *wer* eine Verbindung initiieren darf (Antwortpakete derselben Verbindung werden von Stateful-Firewalls zurückgelassen).
* **Rolle statt Gerät:** Ein Backend-Server ist **Server** für den Browser des Kunden – und gleichzeitig **Client (Initiator)** für die Datenbank, den Mailserver oder ein SMS-Gateway.
* **Ports (Zimmernummern im Hotel):** Auf einem Rechner können mehrere Dienste laufen (z. B. HTTPS `443`, PostgreSQL `5432`). Der Server lauscht auf einem festen Dienstport, der Client erhält vom Betriebssystem einen zufälligen hohen **Ephemeral Port**.

#### Praxis-Check: Eigener Webserver & `netstat`
```powershell
# Im leeren Testordner (nur lokal auf Loopback binden!):
python -m http.server 8000 --bind 127.0.0.1

# Im Browser http://127.0.0.1:8000 aufrufen → Terminal zeigt "GET / HTTP/1.1" 200
# In zweitem Terminal aktive Sockets beobachten:
netstat -an | findstr 8000
```
* **`LISTENING` / `ABHÖREN`** (`127.0.0.1:8000`): Server wartet auf eingehende Anfragen.
* **`ESTABLISHED` / `HERGESTELLT`**: Offene Verbindung zwischen Client-Port (hoch/zufällig) und Server-Port `8000`.
* **`TIME_WAIT` / `WARTEND`**: Verbindung wurde soeben geschlossen, Ports sind noch kurz ablesbar.

---

### 2. Drei-Schichten-Architektur (*3-Tier Architecture*)

| Schicht | Aufgabe | Typischer Standort |
| :--- | :--- | :--- |
| **Präsentation (*Frontend*)** | Darstellung (Kalender, Formulare), Eingaben entgegennehmen, einfache Plausibilitätsprüfung für schnelle Rückmeldung. | Browser oder App auf dem Endgerät des Nutzers. |
| **Anwendungslogik (*Backend*)** | Geschäftsregeln, Rechteprüfung, Doppelbuchungen verhindern, Eingaben final validieren, externe Dienste anstoßen, Web-API bereitstellen. | Anwendungs-/Webserver (Hoster, Rechenzentrum, DMZ). |
| **Datenhaltung (*Datenbank*)** | Persistente Speicherung, Konsistenz/Integrität, Beantwortung von Abfragen. | Abgeschotteter Datenbankserver im internen Netz. |

> **Zwei goldene Architektur-Regeln:**
> 1. **Das Frontend spricht NIEMALS direkt mit der Datenbank!** Eine direkt aus dem Internet erreichbare Datenbank ist eine kritische Sicherheitslücke.
> 2. **Eingaben IMMER auch im Backend validieren!** Das Frontend läuft auf einem fremden Gerät und kann jederzeit manipuliert oder umgangen werden.

---

### 3. Externe Dienste, Web-APIs & zeitgesteuerte Abläufe
* **Web-API:** Schnittstelle meist über **HTTPS (Port 443)** mit **JSON** als Datenformat.
* **E-Mail-Versand:** Backend $\rightarrow$ Mailserver über **SMTP Submission (Port 587)** oder HTTPS-API eines Versanddienstes.
* **SMS-Gateway:** Backend $\rightarrow$ HTTPS-API des SMS-Anbieters (das Backend funkt nie direkt ans Mobilfunknetz).
* **Fremde Fachsoftware:** Immer über deren **offizielle API** anbinden – niemals direkt auf fremde interne Datenbanktabellen zugreifen!
* **Zeitgesteuerte Prozesse (*Scheduler / Cronjob*):** Z. B. Terminerinnerung 24 h vorher $\rightarrow$ **Initiator ist das Backend selbst** (kein Mensch klickt).
* **Webhooks / Rückmeldungen:** Meldet z. B. ein Zahlungsdienst das Ergebnis aktiv ans Backend zurück, entsteht eine **eigene Verbindung** mit dem externen Dienst als **Initiator**!
* **3 Pflichtfragen bei jedem externen Dienst:**
  1. Welche Daten verlassen das eigene System? *(Besondere Vorsicht bei Gesundheitsdaten nach **Art. 9 DSGVO**!)*
  2. Was passiert, wenn der externe Dienst ausfällt? *(Fehlerbehandlung / Warteschlange)*
  3. Wer betreibt den Dienst und wo stehen die Server? *(AVV nach Art. 28 DSGVO, EU/Drittland)*

---

### 4. Kommunikationsmatrix & Kommunikationsdiagramm

#### Die 7 Spalten der Kommunikationsmatrix
| Nr. | Von (Initiator) | Nach (Ziel) | Zweck | Protokoll / Port | Netzgrenze | Status |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | Browser (Patient) | Buchungs-Backend | Freie Termine lesen, Buchung absenden | HTTPS / 443 | Internet $\rightarrow$ Hoster | `belegt` (M1-A) |
| **2** | Buchungs-Backend | Datenbank | Buchungen speichern | PostgreSQL / 5432 | keine (intern) | `Annahme` |
| **3** | Buchungs-Backend | Mail-Dienst | Bestätigung + Absagelink senden | SMTP / 587 | Hoster $\rightarrow$ Internet | `Annahme` |
| **4** | Buchungs-Backend | SMS-Gateway | Erinnerung 24 h vorher (Cronjob) | HTTPS / 443 | Hoster $\rightarrow$ Internet | `belegt` (M1-B) |
| **5** | Buchungs-Backend | Praxisserver | Terminabgleich mit Fachsoftware | *offen* | Hoster $\rightarrow$ Praxisnetz (**kritisch!**) | `offen` |

#### 5 Regeln für das Kommunikationsdiagramm
1. Jeden **Netzbereich / Standort als Rahmen** zeichnen (*Internet*, *Webhoster*, *Praxisnetz*, *Externe Dienste*) – Rahmenkanten = **Netzgrenzen (Firewall)**.
2. Jedes **System als Kästchen** im passenden Rahmen platzieren.
3. Pro Verbindung genau **einen Pfeil vom Initiator zum Ziel** zeichnen (**Antworten erhalten keinen eigenen Pfeil**).
4. Pfeil mit **Matrix-Nummer** + **Protokoll** beschriften.
5. **Annahmen und offene Verbindungen gestrichelt** zeichnen.

---

### 5. Anforderungen sauber analysieren (Beleg-Disziplin)

| Bestandteil einer Kundenaussage | Bedeutung | Beispiel |
| :--- | :--- | :--- |
| **Beobachtung / Problem** | Wahrgenommene Schwierigkeit | *„Im Meeting lagen zwei unterschiedliche Versionsstände vor.“* |
| **Ursachenvermutung** | Unbewiesene Erklärung | *„Das liegt bestimmt am langsamen Server.“* |
| **Lösungsidee** | Vorschneller Produkt-/Kaufwunsch | *„Wir müssen einen schnelleren Server kaufen.“* |
| **Überprüfbare Anforderung** | Ergebnisorientiertes Soll (*funktional* oder *nichtfunktional*) inkl. Prüfansatz | *„Für jeden Auftrag muss eindeutig erkennbar sein, welche Fassung freigegeben ist.“* |

* **Beleg-Disziplin:** Strikt unterscheiden zwischen **`belegt`** (mit Quelle), **`Annahme / Vorschlag`** (explizit markiert) und **`offen`** (als priorisierte Rückfrage an die passende Rolle inkl. Angabe, welche Entscheidung davon abhängt!).
* **2 Praxis-Grundsätze:** (1) Eine technische Anschaffung löst kein fehlendes Organisations-/Freigabekonzept. (2) *„Backup-Job eingerichtet“* $\neq$ *„Wiederherstellung (Restore-Test) nachgewiesen“*.

---

### 🎯 Meilenstein (Modul 1)
* Vollständige **Kommunikationsmatrix** (mit Quellen & Status `belegt`/`Annahme`/`offen`) + **Kommunikationsdiagramm** inkl. Netzgrenzen, Variantenvergleich (*Backend beim Webhoster vs. lokal am Hauptsitz*) und priorisierten Rückfragen.
* *(Siehe auch Querverweis ins 1. LJ: [LF 5 – Teil 7](../../1%20-%20LJ/05_LF5_Software_zur_Datenverwaltung_anpassen/07_Architektur_Kommunikationsmatrix_Sockets_und_Protokolle.md) & [BWL/PM – Teil 1](../../1%20-%20LJ/07_BWL_Projektmanagement_Vertragsrecht_und_Kalkulation/01_Projektmanagement_SMART_Phasen_Lasten_Pflichtenheft.md))*
