# 📱 LS 1 – Thema 14: Smartphone-Schnittstellen (SIM vs. eSIM, Bluetooth & NFC)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] **SIM-Karte vs. eSIM:** Funktionsweise, Vor- und Nachteile im Vergleich
- [ ] **Bluetooth:** Adressen (48-Bit MAC), Übertragungsmodus (**FHSS**), Pairing, Versionen & BLE
- [ ] **NFC (*Near Field Communication*):** Technik, Reichweite, Anwendungen & Beispiele aus dem Alltag

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Klassische SIM-Karte vs. eSIM (*embedded SIM*)
* **Aufgabe der SIM (*Subscriber Identity Module*):** Speichert auf einem gesicherten Krypto-Mikrochip die weltweit eindeutige Teilnehmerkennung (**IMSI**) und den geheimen Authentifizierungsschlüssel (**$K_i$**), mit dem sich das Smartphone gegenüber dem Mobilfunkmast (LTE/5G) ausweist.

| Kriterium | Klassische SIM-Karte (Nano-SIM) | eSIM (*embedded SIM*) |
| :--- | :--- | :--- |
| **Bauform** | Herausnehmbare Kunststoffkarte in einem mechanischen SIM-Kartenschlitten. | Winziger Chip ($6 \times 5\text{ mm}$), der **fest auf dem Smartphone-Mainboard verlötet** ist. |
| **Tarifwechsel / Aktivierung** | Neue Plastikkarte muss per Post verschickt und physisch umgesteckt werden. | Mobilfunkprofil wird **sofort digital per QR-Code oder App (*Over-the-Air*)** auf den Chip geladen! |
| **Mehrere Tarife (Dual-SIM)** | Nur möglich, wenn das Gehäuse Platz für 2 physische Karten bietet. | Es können **5 bis 10+ Profile gleichzeitig gespeichert** (und meist 2 gleichzeitig aktiv genutzt) werden – ideal für *Geschäftlich + Privat* auf einem Gerät oder Reise-Tarife im Ausland! |
| **Platzbedarf & Sicherheit** | SIM-Slot verbraucht viel Platz im Gehäuse und ist eine Schwachstelle für Wasser/Staub. | Spart Platz im Gehäuse (größerer Akku möglich, leichtere IP68-Abdichtung); **hoher Diebstahlschutz**: Ein Dieb kann die eSIM nicht einfach aus dem gesperrten Handy ziehen, um die Ortung per Mobilfunk zu unterbinden! |

---

### 2. Bluetooth (IEEE 802.15.1 – WPAN)
* **Bluetooth-Adresse (`BD_ADDR`):**
  * Jedes Bluetooth-Gerät besitzt eine eindeutige **48-Bit-Hardwareadresse (6 Byte, hexadezimal geschrieben wie eine Ethernet-MAC-Adresse: `00:11:22:33:AA:BB`)**, bestehend aus 24 Bit Herstellerkennung (OUI) und 24 Bit Gerätekennung. *(Aus Datenschutzgründen nutzen moderne Smartphones zusätzlich zufällig wechselnde **Random Private Addresses**, um Tracking in Kaufhäusern zu verhindern!)*
* **Übertragungsmodus & Frequenzsprungverfahren (**FHSS** – Wichtig für die KA!):**
  * Bluetooth funkt im lizenzfreien **2,4-GHz-ISM-Band** ($2{,}402\text{ bis }2{,}480\text{ GHz}$) – also im **exakt selben Frequenzbereich wie 2,4-GHz-WLAN und Mikrowellen**!
  * Warum stören sich Bluetooth und WLAN trotzdem kaum? Weil Bluetooth **FHSS (*Frequency Hopping Spread Spectrum*)** nutzt: Das Frequenzband ist in **79 schmale Kanäle** (je $1\text{ MHz}$ breit; bei BLE 40 Kanäle à $2\text{ MHz}$) unterteilt, und Sender sowie Empfänger **wechseln bis zu 1.600-mal pro Sekunde synchron den Kanal** (*Adaptive Frequency Hopping* überspringt dabei gezielt Kanäle, die gerade von einem WLAN belegt sind!).
* **Pairing (Kopplung) & Betriebsmodi:**
  * Beim **Pairing** tauschen zwei Geräte einmalig kryptografische Schlüssel (*Link Key*) aus (Authentifizierung per Zahlenvergleich / Passkey oder *Just Works*), damit die Verbindung danach verschlüsselt ist und sich automatisch wiederverbindet.
  * **Piconet (Master-Slave-Topologie):** Ein zentrales Gerät (**Master**, z. B. Smartphone) kann gleichzeitig mit bis zu **7 aktiven Slaves** (Kopfhörer, Smartwatch, Maus, Tastatur) in einem *Piconet* kommunizieren.
  * **Bluetooth Classic (BR/EDR) vs. Bluetooth Low Energy (BLE ab BT 4.0 / 5.0 / 5.3 / 5.4):**
    * **Classic:** Für kontinuierliche Datenströme mit höherer Bandbreite (Audio-Streaming zu Kopfhörern, Dateiübertragung).
    * **BLE (*Low Energy*):** Für Geräte, die nur alle paar Sekunden winzige Datenpakete senden (Pulsmesser, Smartwatches, Smart-Home-Sensoren, **Apple AirTags / Smart-Tracker**) und mit einer einzigen Knopfzelle **1 bis 2 Jahre** durchhalten!

---

### 3. NFC (*Near Field Communication*)
* **Technisches Prinzip:**
  * Basiert auf **elektromagnetischer Induktion** bei einer Frequenz von **$13{,}56\text{ MHz}$** (Weiterentwicklung von HF-RFID).
  * **Extrem kurze Reichweite von maximal $1\text{ bis }4\text{ cm}$**!
* **Die 3 Betriebsmodi und ihre Alltagsbeispiele:**
  1. **Card Emulation Mode (Kartenemulation):** Der NFC-Chip im Smartphone (mit eigenem Sicherheitschip *Secure Element*) verhält sich gegenüber einem Kassenterminal exakt wie eine kontaktlose Bankkarte oder ein Mitarbeiterausweis *(Beispiele: **Apple Pay, Google Pay**, digitales Deutschlandticket, elektronischer Hotelzimmerschlüssel, Zeiterfassungsterminal)*.
  2. **Reader / Writer Mode (Lesen & Schreiben):** Das Smartphone erzeugt selbst ein magnetisches Wechselfeld, versorgt damit einen batterielosen (**passiven**) NFC-Chip mit Strom und liest ihn aus *(Beispiele: **elektronischen Personalausweis (eID) oder elektronische Gesundheitskarte (eGK)** mit dem Handy auslesen, Smart-Poster/Visitenkarten scannen)*.
  3. **Peer-to-Peer Mode:** Zwei aktive Geräte tauschen beim Aneinanderhalten Verbindungsdaten aus *(Beispiel: **Schnelles Bluetooth-/WLAN-Pairing** bei einem tragbaren Lautsprecher oder Kamera durch kurzes Antippen mit dem Handy)*.

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Warum gilt die extrem geringe Reichweite von NFC (unter 4 cm) beim kontaktlosen Bezahlen im Supermarkt nicht als technischer Nachteil, sondern als entscheidendes Sicherheitsmerkmal gegenüber Bluetooth?“*
   * **Lösung:** Würde das Bezahlen über Bluetooth ($10\text{–}100\text{ m}$ Reichweite) laufen, könnte ein Angreifer im Gedränge aus mehreren Metern Entfernung unbemerkt eine Funkverbindung aufbauen (*Man-in-the-Middle / Relay-Attack*) oder es könnte versehentlich die Zahlung an der falschen Nachbarkasse ausgelöst werden. Durch die physikalische Begrenzung von NFC auf **wenige Zentimeter** ist eine bewusste, gezielte Annäherung des Geräts an das Terminal zwingend erforderlich (**physische Willensbestätigung + Abhörschutz**).
2. **Frage 2:** *„Wie schafft es ein Bluetooth-Headset, mitten in einem Großraumbüro mit 15 überlasteten 2,4-GHz-WLAN-Routern trotzdem unterbrechungsfrei den Ton zu übertragen?“*
   * **Lösung:** Durch das **adaptive Frequenzsprungverfahren (AFH / FHSS)**: Bluetooth bleibt nicht wie ein WLAN-Router starr auf einem einzigen breiten Kanal sitzen, sondern **springt 1.600-mal pro Sekunde über bis zu 79 schmale Kanäle** hin und her und erkennt automatisch, welche Frequenzen durch ein WLAN gestört sind, um diese beim Springen auszulassen!
