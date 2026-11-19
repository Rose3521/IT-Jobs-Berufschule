# 💻 LF 9 – Modul 6: Transport- & Anwendungsprotokolle, Sockets & Netzwerkprogrammierung in C#

> **Lernsituation:** *Kurierkollektiv Speichenwerk* (Fahrer-App & Auftragsserver „Speichenwerk Dispo“ – Prototyp SWP 0.1 + Funklöcher in Tiefgaragen)  
> **EduSysTrainings-Status:** Einheit `06-04` ✅ vollständig eingearbeitet · Einheiten `06-01`, `06-02`, `06-03` & `06-05` 🚧 noch in Vorbereitung (TCP/UDP- & HTTP/TLS-Grundwissen ergänzt)

---

## ✅ Checkliste der Lehrinhalte (Modul 6)
- [ ] **TCP vs. UDP** anwendungsbezogen begründen (Aufträge/Zustellnachweise $\rightarrow$ TCP; zyklische GPS-Positionen $\rightarrow$ UDP)
- [ ] **HTTP-Aufbau, Methoden (`GET/POST/PUT/DELETE`), Statuscodes (`2xx/4xx/5xx`)** & **HTTPS/TLS-Zertifikate** erläutern
- [ ] **Socket & Endpunkt (`IP:Port`)** sowie die .NET-Klassen **`TcpListener`, `TcpClient`, `NetworkStream`, `StreamReader`/`StreamWriter`** anwenden
- [ ] Erklären, warum **TCP ein Byte-Datenstrom** ist und ein Anwendungsprotokoll **Nachrichtengrenzen (`\n`/Länge), Kodierung (UTF-8), Statuscodes, Zustand & Idempotenz** festlegen muss
- [ ] Einen **nebenläufigen TCP-Server** (`async` / `Task.Run`) schreiben und gemeinsame Daten mit **`lock`** vor Race Conditions schützen
- [ ] Einen **funklochfesten Client** mit sauberer Fehlerbehandlung (`SocketException`, `IOException`, `ReadTimeout`, `ReadLine() == null`) und dem Muster ***„erst merken, dann senden“*** (`Queue<string>`) umsetzen
- [ ] Den Unterschied von **`UdpClient`** (*Fire-and-Forget*) gegenüber TCP im Praxistest erklären

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. TCP vs. UDP & HTTP/HTTPS-Grundlagen *(Überblick für `06-01`–`06-03` & `06-05`)*
* **TCP (verbindungsorientiert):** 3-Way-Handshake (`SYN` / `SYN-ACK` / `ACK`), garantierte Reihenfolge, Bestätigung & erneute Übertragung verlorener Segmente $\rightarrow$ **Pflicht für Aufträge, Zustellnachweise, Web-APIs**.
* **UDP (verbindungslos):** Kein Handshake, minimaler Header (8 Byte), keine Wiederholung verlorener Pakete $\rightarrow$ ideal für **kurzlebige Echtzeitdaten** (z. B. GPS-Position alle 10 s: fehlt eine, kommt gleich die nächste!).
* **Wichtige HTTP-Statuscodes (auch als Vorbild für eigene Protokolle):**
  * **`200 OK`** / **`201 Created`** – Erfolg
  * **`400 Bad Request`** – Syntaxfehler / unbekannter Befehl
  * **`401 Unauthorized`** – Anmeldung fehlt / unbekannter Benutzer
  * **`403 Forbidden`** – Zugriff nicht erlaubt (z. B. Befehl vor `HALLO`)
  * **`404 Not Found`** – Ressource / Auftrag nicht gefunden
  * **`409 Conflict`** – Zustands-Konflikt (z. B. fremder Auftrag)
  * **`503 Service Unavailable`** – Server vorübergehend überlastet/nicht bereit $\rightarrow$ Client sollte mit Backoff wiederholen!
* **HTTPS / TLS:** Schützt Vertraulichkeit (Verschlüsselung), Integrität und Server-Authentizität (X.509-Zertifikat).

---

### 2. Sockets & Design eines eigenen Anwendungsprotokolls (Beispiel: SWP 0.1)
Ein **Socket** ist die „Netzwerk-Steckdose“ eines Programms mit einem **Endpunkt aus IP + Port** (z. B. `127.0.0.1:5050`).

#### Warum braucht man ein Anwendungsprotokoll über TCP?
TCP liefert nur einen **fortlaufenden Byte-Strom**, keine getrennten Pakete! Sendet der Client zweimal 10 Byte, liest der Server eventuell einmal 20 Byte oder dreimal Bruchstücke. Ein Protokoll legt daher **6 Dinge** fest:
1. **Nachrichtengrenzen (*Framing*):** Trennzeichen (z. B. jede Nachricht endet mit **`\n`**) oder vorangestellte Längenangabe.
2. **Zeichenkodierung:** Verbindlich **UTF-8** (sonst kaputte Umlaute wie *„Süd“*).
3. **Request / Response:** Genau 1 Antwort pro Befehl.
4. **Maschinenlesbare Statuscodes:** Zahl fürs Programm (`401`), Klartext für Menschen (`UNBEKANNTER FAHRER`).
5. **Zustand (*Stateful*):** Server merkt sich pro Verbindung, wer angemeldet ist (`HALLO F01` muss vor `AUFTRAG?` kommen).
6. **Idempotenz (entscheidend bei Funklöchern!):**
   * *Definition:* Mehrfaches Ausführen einer Operation führt zum **identischen Ergebnis** wie einmaliges Ausführen.
   * *Beispiel:* Geht die Antwort `OK A1001` auf `ZUGESTELLT A1001` in einer Tiefgarage verloren, schickt der Client den Befehl erneut $\rightarrow$ der Server antwortet nochmals `OK A1001`, **bucht aber nichts doppelt**!

> **Sicherheitshinweis:** Ein eigenes Klartext-TCP-Protokoll ist **unverschlüsselt** und ohne starke Authentifizierung – für Produktivdaten (z. B. Zustellnachweise) ungeeignet! Test-Server daher ausschließlich an **`127.0.0.1` (`IPAddress.Loopback`)** binden und später auf **HTTPS/TLS** umstellen.

---

### 3. Netzwerkprogrammierung in C# / .NET (`System.Net.Sockets`)

| Klasse | Aufgabe | Wichtige Methoden / Optionen |
| :--- | :--- | :--- |
| **`TcpListener`** | Server: lauscht auf IP & Port | `Start()`, `AcceptTcpClient()` / `AcceptTcpClientAsync()` |
| **`TcpClient`** | Eine TCP-Verbindung (Client & Server) | `Connect(host, port)`, `GetStream()` |
| **`NetworkStream`** | Byte-Datenstrom der Verbindung | `ReadTimeout` (z. B. `5000` ms) |
| **`StreamReader` / `StreamWriter`** | Zeilenweises Lesen/Schreiben (UTF-8) | `ReadLineAsync()`, `WriteLineAsync()`, **`AutoFlush = true`**, **`NewLine = "\n"`** |

#### Nebenläufiger Server mit `async` / `Task.Run` und `lock` (gegen Race Conditions!)
Ohne Parallelisierung blockiert Client 1 alle anderen Fahrer. Mit `Task.Run` läuft jeder Client parallel – und **`lock`** verhindert, dass zwei Fahrer gleichzeitig denselben Auftrag aus einer gemeinsamen Liste ziehen:

```csharp
var sperre = new object();
var listener = new TcpListener(IPAddress.Loopback, 5050);
listener.Start();

while (true)
{
    TcpClient client = await listener.AcceptTcpClientAsync();
    _ = Task.Run(() => BearbeiteClientAsync(client));
}

async Task BearbeiteClientAsync(TcpClient client)
{
    using (client)
    using (NetworkStream stream = client.GetStream())
    using (var reader = new StreamReader(stream, Encoding.UTF8))
    using (var writer = new StreamWriter(stream, new UTF8Encoding(false)) { AutoFlush = true, NewLine = "\n" })
    {
        string? zeile;
        while ((zeile = await reader.ReadLineAsync()) != null)
        {
            string antwort;
            lock (sperre)
            {
                // Kritischer Abschnitt: gemeinsame Auftragsdaten threadsicher lesen/ändern
                antwort = VerarbeiteBefehl(zeile.Trim());
            }
            await writer.WriteLineAsync(antwort);
            if (antwort == "OK TSCHUESS") break;
        }
    }
}
```

---

### 4. Funklochfester Client: Fehlerbehandlung & *„erst merken, dann senden“*

| Situation im Client | Verhalten in C# / .NET |
| :--- | :--- |
| Server läuft nicht / falscher Port | **`SocketException`** bei `Connect(...)` |
| Verbindung bricht ab (Funkloch / Serverstopp) | **`IOException`** beim Lesen/Schreiben |
| Server antwortet nicht rechtzeitig | **`IOException`** nach Ablauf von `stream.ReadTimeout` |
| Server beendet Verbindung sauber | `reader.ReadLine()` liefert **`null`** |

* **Muster „erst merken, dann senden“ (`Queue<string>`):**
  1. Zustellung erst lokal in `offeneMeldungen.Enqueue(auftragsNr)` sichern.
  2. Senden – und erst bei Server-Antwort **`OK`** per `Dequeue()` entfernen!
  3. Bei Verbindungsabbruch kurz warten (z. B. 3 s), neu verbinden, neu anmelden (`HALLO`) und alle offenen Einträge der Queue nachliefern.
* **Persistenz-Falle bei Server-Neustart:** Speichert der Server Aufträge nur im Arbeitsspeicher (`List`/`Dictionary`), gehen Statusänderungen bei einem Neustart verloren $\rightarrow$ im Echtbetrieb **Datenbank-Persistenz**!
* **Test mit `UdpClient`:** Sendet ohne Verbindungsaufbau (`Send`/`Receive`). Läuft kein Empfänger, meldet der Sender meist **keinen Verbindungsfehler** – das Paket geht einfach verloren.

---

### 🎯 Meilenstein (Modul 6)
* Lauffähiger **C#-Prototyp (SWP-Server + funklochfester Fahrer-Client)** inkl. Testprotokoll (Testfälle T1–T9, Parallelbetrieb, Server-Neustart-Analyse) und TCP/UDP-Begründung.
* *(Querverweis 1. LJ: [LF 5 – Teil 7](../../1%20-%20LJ/05_LF5_Software_zur_Datenverwaltung_anpassen/07_Architektur_Kommunikationsmatrix_Sockets_und_Protokolle.md))*
