# 🔌 LF 5 – Teil 7: Systemarchitektur, Kommunikationsmatrix, Sockets & eigene Protokolle

> **Lernfeld 5 (mit Übergang zu LF 9 / AP2 – relevant für alle IT-Berufe):** Software zur Verwaltung von Daten anpassen & Anwendungen ins Netzwerk einbinden  
> **Praxis- & Prüfungsfokus:** *Client-Server-Rollen, 3-Schichten-Architektur, Web-APIs, Kommunikationsmatrix mit Netzgrenzen, TCP-Sockets in C#/.NET, Protokolldesign, Nebenläufigkeit (`async`/`lock`) & Idempotenz*

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Client und Server** als *Rollen* (nicht Geräte!) unterscheiden und den **Initiator** einer Verbindung bestimmen
- [ ] Einen lokalen Test-Webserver starten (`python -m http.server`) und Server-/Client-Ports mit `netstat` auslesen
- [ ] Die **Drei-Schichten-Architektur** (*Präsentation / Frontend, Anwendungslogik / Backend, Datenhaltung / Datenbank*) und ihre Sicherheitsregeln erklären
- [ ] **Externe Dienste & Web-APIs** (SMTP, SMS-Gateway, Fachsoftware-API, Cronjob/Scheduler) einbinden und datenschutzrechtlich prüfen
- [ ] Eine **Kommunikationsmatrix** (inkl. Netzgrenze & Nachweisstatus) und ein **Kommunikationsdiagramm** erstellen
- [ ] **Sockets & eigene Anwendungsprotokolle** erklären (TCP-Byte-Datenstrom, Nachrichtengrenzen, UTF-8, Statuscodes, Zustand, **Idempotenz**)
- [ ] Einen **TCP-Server und -Client in C# (`System.Net.Sockets`)** inkl. Nebenläufigkeit (`Task.Run` + `lock`), Fehlerbehandlung (`SocketException`/`IOException`) und *„erst merken, dann senden“* (Warteschlange) verstehen sowie TCP vs. `UdpClient` vergleichen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Client-Server-Prinzip: Rollen statt Geräte
Fast jede Netzwerkkommunikation folgt dem **Client-Server-Modell**: Ein **Client** sendet eine Anfrage (*Request*), ein **Server** bietet einen Dienst an, wartet passiv auf einem Port und sendet eine Antwort (*Response*).

* **Initiator-Prinzip (wichtig für Firewalls!):** Der **Client** baut die Verbindung aktiv auf – er ist der **Initiator**. Firewall-Regeln prüfen fast immer, *wer* eine Verbindung initiieren darf (Antwortpakete derselben Verbindung werden von Stateful-Firewalls automatisch zurückgelassen).
* **Rolle statt Gerät:** Client und Server sind **Software-Rollen**, keine physischen Computer! Ein **Backend-Server** ist z. B.:
  * **Server** gegenüber dem Browser des Nutzers, und gleichzeitig
  * **Client (Initiator)** gegenüber der Datenbank, einem Mailserver oder einer externen Web-API.
* **Ein Rechner, viele Dienste (Ports):** Auf einem Server können Webserver (`443`) und Datenbank (`5432`) parallel laufen. Der **Port** adressiert den richtigen Dienst – wie eine Zimmernummer im Hotel. Der Server nutzt einen festen **Well-Known-/Dienstport**, der Client bekommt vom Betriebssystem einen zufälligen hohen **Ephemeral Port** (z. B. `52418`).

#### Praxis-Check: Lokaler Webserver & `netstat` (Windows)
```powershell
# 1. Kleinen Webserver nur lokal (Loopback!) in einem Testordner starten:
python -m http.server 8000 --bind 127.0.0.1

# 2. Im Browser http://127.0.0.1:8000 öffnen (Terminal zeigt z. B. "GET / HTTP/1.1" 200)

# 3. In einem zweiten Terminal aktive Sockets und Ports beobachten:
netstat -an | findstr 8000
```
* **`LISTENING` / `ABHÖREN`** auf `127.0.0.1:8000`: Der Server wartet auf eingehende Verbindungen.
* **`ESTABLISHED` / `HERGESTELLT`**: Aktive Verbindung zwischen `127.0.0.1:<hoher Client-Port>` und `127.0.0.1:8000`.
* **`TIME_WAIT` / `WARTEND`**: Die Verbindung wurde gerade geschlossen (z. B. nach Auslieferung der HTML-Seite); die beteiligten Ports sind noch kurz sichtbar.

---

### 2. Drei-Schichten-Architektur einer Webanwendung (*3-Tier Architecture*)

Moderne Anwendungen trennen Darstellung, Geschäftslogik und Datenspeicherung in **3 Schichten**. Jede Schicht spricht nur mit ihren direkten Nachbarn:

| Schicht | Aufgabe | Typischer Ausführungsort |
| :--- | :--- | :--- |
| **1. Präsentation (*Frontend*)** | Zeigt Daten an und nimmt Benutzereingaben entgegen (z. B. Formulare, Kalender, Validierung für schnelles Feedback). | Im **Browser** (HTML/CSS/JS) oder in einer **Mobile/Desktop-App** auf dem Endgerät des Nutzers. |
| **2. Anwendungslogik (*Backend*)** | Setzt fachliche Regeln um: prüft Berechtigungen, validiert Eingaben, verhindert Doppelbuchungen, ruft externe Dienste auf, bietet dem Frontend meist eine **Web-API**. | Auf einem **Anwendungs-/Webserver** (im Rechenzentrum, beim Hoster oder in der DMZ). |
| **3. Datenhaltung (*Datenbank*)** | Speichert Daten dauerhaft (persistent), sichert Integrität und liefert Daten auf Abfrage. | Auf einem abgeschotteten **Datenbankserver** im internen Netz (nur für das Backend erreichbar!). |

> **Zwei goldene Sicherheitsregeln der 3-Schichten-Architektur:**
> 1. **Das Frontend spricht NIEMALS direkt mit der Datenbank!** Eine aus dem Internet erreichbare Datenbank ist ein kritisches Sicherheitsrisiko.
> 2. **Eingaben IMMER auch im Backend validieren!** Eine Prüfung im Frontend (z. B. E-Mail-Format oder Pflichtfelder per JavaScript) dient nur der Benutzerfreundlichkeit – sie läuft auf dem Gerät des Nutzers und lässt sich jederzeit umgehen oder manipulieren.

---

### 3. Externe Dienste, Web-APIs & zeitgesteuerte Prozesse

Funktionen wie E-Mail-Versand, SMS, Zahlungen oder Kartendienste programmiert man nicht selbst, sondern bindet sie über eine **Schnittstelle (API – *Application Programming Interface*)** an:
* **Web-API:** Nimmt Anfragen meist verschlüsselt per **HTTPS (Port 443)** entgegen und tauscht Daten im **JSON-Format** aus.
* **E-Mail-Versand:** Das Backend übergibt Mails per **SMTP-Submission (Port 587)** oder per HTTPS-API an einen Mailserver/Versanddienst.
* **SMS-Gateway:** Das Backend sendet Rufnummer und Text per HTTPS an den Anbieter – das Backend kommuniziert nie direkt mit dem Mobilfunknetz.
* **Fremde Fachsoftware anbinden:** Immer über deren **offizielle API** lesen/schreiben – niemals direkt in deren fremde Datenbank eingreifen!
* **Zeitgesteuerte Abläufe (*Scheduler / Cronjob*):** Manche Aktionen löst kein Mensch aus (z. B. Terminerinnerung 24 h vorher). Ein geplanter Hintergrundprozess im Backend stößt die Verbindung an – **Initiator ist dann das Backend selbst**.
* **Webhook / Rückmeldung (Achtung bei der Analyse!):** Zahlungs- oder Versanddienste melden Ergebnisse oft aktiv an das Backend zurück. Das ist eine **eigene Verbindung** mit dem externen Dienst als **Initiator**!

> **3 Pflichtfragen vor jeder Anbindung eines externen Dienstes (DSGVO & Betrieb):**
> 1. **Welche Daten verlassen das eigene System?** *(Besondere Vorsicht bei besonders schützenswerten Daten nach Art. 9 DSGVO, z. B. Gesundheitsdaten!)*
> 2. **Was passiert, wenn der externe Dienst nicht erreichbar ist?** *(Fehlerbehandlung, Warteschlange, Fallback)*
> 3. **Wer betreibt den Dienst und wo stehen die Server?** *(AVV nach Art. 28 DSGVO, EU/Drittland)*

---

### 4. Kommunikationsmatrix & Kommunikationsdiagramm

Bevor Firewall-Regeln konfiguriert oder Schnittstellen programmiert werden, dokumentiert man alle Verbindungen systematisch als **Duo aus Tabelle (Vollständigkeit) und Diagramm (Überblick)**:

#### A. Aufbau einer Kommunikationsmatrix (7 Spalten)

| Nr. | Von (Initiator) | Nach (Ziel) | Zweck | Protokoll / Port | Netzgrenze | Status (Beleg-Disziplin) |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | Browser (Kunde) | Web-Backend | Seite laden, Buchung absenden | HTTPS / 443 | Internet $\rightarrow$ Hoster | `belegt` (z. B. Quelle M1-A) |
| **2** | Web-Backend | Datenbank | Termine lesen & speichern | PostgreSQL / 5432 | keine (intern) | `Annahme` (DB-Typ) |
| **3** | Web-Backend | Mail-Dienst | Bestätigungsmail versenden | SMTP / 587 | Hoster $\rightarrow$ Internet | `Annahme` (SMTP statt API) |
| **4** | Web-Backend | Praxisserver | Termin abgleichen | *offen* | Hoster $\rightarrow$ Praxisnetz (**kritisch!**) | `offen` (Firewall klären) |

* **Nachweisstatus konsequent trennen:**
  * **`belegt`**: Steht explizit im Kundenmaterial / Lastenheft (Quelle angeben!).
  * **`Annahme`**: Eigener fachlicher Vorschlag (muss bestätigt werden).
  * **`offen`**: Fehlt noch – wird direkt in eine **Rückfrage an den Kunden / Dienstleister** übersetzt!

#### B. 5 Regeln für ein sauberes Kommunikationsdiagramm
1. Jeden **Netzbereich / Ort als Rahmen** zeichnen (z. B. *Internet*, *Webhoster*, *Firmennetz*, *Externe Anbieter*) – die Rahmenkanten sind die **Netzgrenzen (Firewall-Übergänge)**.
2. Jedes **System als Kästchen** in seinen Rahmen setzen.
3. Pro Verbindung **einen Pfeil vom Initiator zum Ziel** zeichnen (**Antworten bekommen keinen eigenen Pfeil**, sie laufen auf derselben Verbindung zurück!).
4. Jeden Pfeil mit der **Nummer aus der Matrix** und dem Protokoll beschriften.
5. **Angenommene oder offene Verbindungen gestrichelt** darstellen, damit Lücken sofort sichtbar bleiben.

---

### 5. Sockets & Design eigener Anwendungsprotokolle

Ein **Socket** (*„Netzwerk-Steckdose“*) ist der Anschlusspunkt eines Programms ans Netzwerk. Jeder Socket besitzt einen **Endpunkt aus IP-Adresse und Port** (z. B. `127.0.0.1:5050`).

#### Warum reicht TCP allein nicht? (Aufgaben des Anwendungsprotokolls)
TCP garantiert zwar, dass Bytes vollständig und in der richtigen Reihenfolge ankommen – aber **TCP ist ein reiner Byte-Datenstrom**, kein Paketdienst für fertige Nachrichten! Sendet der Client zweimal 10 Byte, kann der Server einmal 20 Byte oder dreimal Bruchstücke lesen. Deshalb muss jedes Anwendungsprotokoll (wie HTTP, SMTP oder ein eigenes Textprotokoll) **6 Dinge eindeutig festlegen**:

1. **Nachrichtengrenzen (*Framing*):** Wo endet eine Nachricht? Typisch über ein **Trennzeichen** (z. B. Zeilenumbruch `\n`) oder eine vorangestellte **Längenangabe**.
2. **Zeichenkodierung:** Beide Seiten müssen dieselbe Kodierung nutzen (Standard: **UTF-8**, sonst entstehen fehlerhafte Umlaute).
3. **Ablauf (Request / Response):** Auf jeden Befehl folgt exakt eine Antwort – der Client weiß immer, worauf er wartet.
4. **Maschinenlesbare Statuscodes (angelehnt an HTTP):** Zahlen für Programme, Klartext für Menschen:
   * **`200` / `OK`** – Anfrage erfolgreich verarbeitet
   * **`400`** – Anfrage syntaktisch falsch / unbekannter Befehl (*Bad Request*)
   * **`401`** – Nicht authentifiziert / unbekannter Benutzer (*Unauthorized*)
   * **`403`** – Nicht erlaubt / vorherige Anmeldung fehlt (*Forbidden*)
   * **`404`** – Ressource / Auftrag nicht gefunden (*Not Found*)
   * **`409`** – Konflikt mit aktuellem Zustand (*Conflict*, z. B. fremder Auftrag)
   * **`503`** – Dienst vorübergehend nicht verfügbar (*Service Unavailable*)
5. **Zustand (*Stateful* vs. *Stateless*):** Merkt sich der Server pro Verbindung, wer angemeldet ist (*Stateful*, z. B. erst `HALLO`, dann `AUFTRAG?`), oder enthält jede Anfrage alle Infos selbst (*Stateless*, wie bei REST/HTTP)?
6. **Idempotenz (Wiederholbarkeit – extrem wichtig bei Mobilfunk / Funklöchern!):**
   * **Definition:** Eine Operation ist **idempotent**, wenn ihr mehrfaches Ausführen zum **exakt selben Ergebnis** führt wie einmaliges Ausführen.
   * **Warum lebenswichtig?** Geht die Server-Antwort `OK A1001` in einer Tiefgarage verloren, sendet der Client `ZUGESTELLT A1001` sicherheitshalber erneut. Ist der Server idempotent, antwortet er nochmals `OK A1001`, **ohne** den Auftrag doppelt zu buchen oder einen Fehler zu werfen!

> **Sicherheitshinweis für Prototypen:** Ein einfaches Textprotokoll über reines TCP ist **unverschlüsselt** (Klartext im Netz mitlesbar!) und ohne echte Authentifizierung. Für echte Produktivdaten (z. B. Zustellnachweise oder Kundendaten) sind **TLS/HTTPS + starke Authentifizierung** Pflicht – lokale Prototypen daher immer nur an **`127.0.0.1` (`IPAddress.Loopback`)** binden!

---

### 6. Netzwerkprogrammierung in C# / .NET (`System.Net.Sockets`)

#### A. Die wichtigsten .NET-Klassen für TCP
| Klasse | Rolle / Aufgabe | Zentrale Methoden / Eigenschaften |
| :--- | :--- | :--- |
| **`TcpListener`** | **Server:** Lauscht passiv auf IP & Port | `Start()`, `AcceptTcpClient()` / `AcceptTcpClientAsync()` |
| **`TcpClient`** | **Eine TCP-Verbindung** (Client- und Serverseite) | `Connect(host, port)`, `GetStream()` |
| **`NetworkStream`** | Der eigentliche **Byte-Datenstrom** | `ReadTimeout`, Lesen/Schreiben meist über `StreamReader`/`StreamWriter` |
| **`StreamReader` / `StreamWriter`** | Komfortable **Zeilen-Verarbeitung** (UTF-8 + `\n`) | `ReadLine()` / `ReadLineAsync()`, `WriteLine()`, **`AutoFlush = true`**, **`NewLine = "\n"`** |

*(Merke: Ohne `AutoFlush = true` puffert der `StreamWriter` Daten eventuell im Arbeitsspeicher, statt sie sofort übers Netz zu schicken!)*

#### B. Mehrere Clients gleichzeitig (`async` / `Task.Run`) & Thread-Sicherheit (`lock`)
Ein einfacher Server mit einer einzelnen `while`-Schleife bedient immer nur **einen Client nach dem anderen** – alle anderen Clients müssen warten!
* **Lösung für Parallelbetrieb:** Jede angenommene Verbindung sofort an eine eigene asynchrone Aufgabe (`Task.Run`) übergeben.
* **Schutz vor Race Conditions (`lock`):** Greifen mehrere Tasks gleichzeitig auf gemeinsame Daten zu (z. B. eine Auftragsliste), könnten zwei Clients versehentlich denselben Auftrag erhalten. Ein **`lock`-Block** garantiert, dass immer nur ein Thread/Task gleichzeitig liest oder ändert:

```csharp
using System.Net;
using System.Net.Sockets;
using System.Text;

var sperre = new object();
var listener = new TcpListener(IPAddress.Loopback, 5050);
listener.Start();

while (true)
{
    TcpClient client = await listener.AcceptTcpClientAsync();
    _ = Task.Run(() => BearbeiteClientAsync(client)); // Parallel pro Client
}

async Task BearbeiteClientAsync(TcpClient client)
{
    using (client)
    using (NetworkStream stream = client.GetStream())
    using (var reader = new StreamReader(stream, Encoding.UTF8))
    using (var writer = new StreamWriter(stream, new UTF8Encoding(false)) { AutoFlush = true, NewLine = "\n" })
    {
        string? zeile;
        while ((zeile = await reader.ReadLineAsync()) != null) // null = Client hat Verbindung geschlossen
        {
            string antwort;
            lock (sperre) // Kritischer Abschnitt: gemeinsame Daten schützen!
            {
                antwort = zeile.Trim() == "ZEIT"
                    ? DateTime.Now.ToString("HH:mm:ss")
                    : "FEHLER 400 UNBEKANNTER BEFEHL";
            }
            await writer.WriteLineAsync(antwort);
        }
    }
}
```

#### C. Robuste Fehlerbehandlung im Client & das Muster *„erst merken, dann senden“*
Im Netzwerk muss der Client jederzeit mit Ausfällen rechnen:

| Fehlersituation | Verhalten in C# / .NET |
| :--- | :--- |
| Server läuft nicht / falscher Port / Firewall lehnt ab | **`SocketException`** beim `Connect(...)` (*Verbindung verweigert*) |
| Verbindung bricht mittendrin ab (z. B. Funkloch, Serverabsturz) | **`IOException`** beim Lesen oder Schreiben |
| Server antwortet nicht innerhalb der Frist | **`IOException`** nach Ablauf von `stream.ReadTimeout` (z. B. `5000` ms) |
| Server schließt die Verbindung sauber | `reader.ReadLine()` liefert **`null`** (keine Exception!) |

* **Funkloch-Muster „erst merken, dann senden“ (mit `Queue<string>`):**
  1. Tritt ein wichtiges Ereignis auf (z. B. Paketzustellung), wird die Meldung **zuerst** in eine lokale Warteschlange gelegt (`offeneMeldungen.Enqueue(nr)`).
  2. Erst wenn der Server die Meldung mit **`OK`** bestätigt hat, wird sie aus der Warteschlange entfernt (`Dequeue()`).
  3. Bei `SocketException` oder `IOException` wartet der Client kurz, verbindet sich neu und schickt alle noch offenen Einträge aus der Warteschlange erneut. Dank **Idempotenz** auf Serverseite entsteht dabei weder Datenverlust noch eine Doppelbuchung!
  4. **Achtung bei Server-Neustart:** Hält ein Server seinen Zustand nur im **RAM** (z. B. `List`/`Dictionary`), sind nach einem Server-Neustart alle bisherigen Statusänderungen verloren $\rightarrow$ Produktivserver müssen Zustände **persistent in einer Datenbank** speichern!

#### D. Wann statt TCP besser UDP (`UdpClient`)?
* Für **kritische Transaktionen** (Aufträge, Zustellnachweise, Buchungen): **Immer TCP** (Verbindungsaufbau, Reihenfolge, automatische Wiederholung verlorener Segmente).
* Für **häufige, kurzlebige Statuswerte** (z. B. GPS-Position alle 10 Sekunden, Live-Telemetrie): **`UdpClient`** (`Send` / `Receive`) ist verbindungslos und spart Overhead. **Besonderheit im Test:** Startet man einen UDP-Sender ohne laufenden Empfänger, wirft `Send` in der Regel **keine** Verbindungsfehlermeldung – das Paket verschwindet einfach still (*Fire-and-Forget*), weil UDP keinen Handshake und keine Empfangsbestätigung kennt.
