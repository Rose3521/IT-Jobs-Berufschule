# 📄 LF 5 – Teil 6: Datenformate (CSV, JSON, XML), Zeichensätze (ASCII, UTF-8) & Softwaretests

> **Lernfeld 5:** Software zur Verwaltung von Daten anpassen  
> **AP1-Statistik:** *ASCII & Dateiformate (6 P.)* in der AP1; in der AP2 FIAE brachten *Testen/Code Coverage (77 P.) sowie XML & JSON (33 P.)* zusammen **110 Punkte**!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Datenformate (CSV, JSON, XML)** nach Struktur, Overhead und Syntax vergleichen und fehlerfrei schreiben
- [ ] **Zeichensätze (ASCII, ISO-8859-1, Unicode / UTF-8)** und ihren Speicherbedarf erklären
- [ ] **Softwaretest-Verfahren:** Black-Box- vs. White-Box-Test, Äquivalenzklassen, Unit-Tests & **Code Coverage ($C_0 / C_1$)** erläutern

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Strukturierte Text-Datenformate: CSV vs. JSON vs. XML

| Kriterium | CSV (*Comma-Separated Values*) | JSON (*JavaScript Object Notation*) | XML (*Extensible Markup Language*) |
| :--- | :--- | :--- | :--- |
| **Aufbau / Struktur** | **Flache Tabelle** (1 Zeile = 1 Datensatz; Spalten getrennt durch `;` oder `,`). | **Hierarchisch (Baumstruktur)** aus Schlüssel-Wert-Paaren `{"key": value}` und Listen `[...]`. | **Hierarchisch (Baumstruktur)** aus Start- und End-Tags `<element attribut="wert">Inhalt</element>`. |
| **Dateigröße / Overhead** | **Minimal** (kompaktestes Format). | **Gering** (sehr leichtgewichtig, schnell zu parsen). | **Hoch** (viele schließende Tags wiederholen die Feldnamen ständig). |
| **Datentypen** | Keine (alles wird als Text interpretiert). | Unterstützt direkt `String` (`"..."`), `Number` (`42`, `19.99`), `Boolean` (`true`/`false`), `Array` (`[...]`), `Object` (`{...}`), `null`. | Textbasiert; strenge Typ- und Strukturprüfung über ein **XML-Schema (XSD)** möglich. |
| **Typischer Einsatz** | Excel-Import/Export, einfacher Massendatenaustausch. | **Standard für moderne Web-APIs (REST)**, Konfigurationsdateien, NoSQL-Datenbanken (MongoDB). | SOAP-Webservices, elektronische Rechnungen (**XRechnung / ZUGFeRD**), Office-Dateien. |

#### Syntax-Vorlage zum direkten Abschreiben in der Prüfung:

**CSV:**
```csv
ArtikelNr;Bezeichnung;Preis;AufLager
101;Monitor 27 Zoll;249.90;true
102;USB-Tastatur;29.50;false
```

**JSON (Achtung: Schlüssel IMMER in doppelten Anführungszeichen `"..."`, Zahlen & `true`/`false` OHNE Anführungszeichen, kein Komma nach dem letzten Element!):**
```json
{
  "lieferant": "IT-Grosshandel GmbH",
  "artikel": [
    {
      "artikelNr": 101,
      "bezeichnung": "Monitor 27 Zoll",
      "preis": 249.90,
      "aufLager": true
    },
    {
      "artikelNr": 102,
      "bezeichnung": "USB-Tastatur",
      "preis": 29.50,
      "aufLager": false
    }
  ]
}
```

**XML (Achtung: Genau 1 Wurzelelement umschließt alles, jedes geöffnete Tag `<tag>` muss exakt wieder geschlossen werden `</tag>`, Attributwerte in `"..."`!):**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<katalog lieferant="IT-Grosshandel GmbH">
  <artikel artikelNr="101">
    <bezeichnung>Monitor 27 Zoll</bezeichnung>
    <preis>249.90</preis>
    <aufLager>true</aufLager>
  </artikel>
  <artikel artikelNr="102">
    <bezeichnung>USB-Tastatur</bezeichnung>
    <preis>29.50</preis>
    <aufLager>false</aufLager>
  </artikel>
</katalog>
```

---

### 2. Zeichensätze: ASCII vs. ISO-8859-1 vs. Unicode (UTF-8) (AP1 Frühjahr 2024)
* **ASCII (*American Standard Code for Information Interchange*):**
  * Nutzt **7 Bit pro Zeichen** $\rightarrow 2^7 = \mathbf{128\text{ Zeichen}}$ (`0` bis `127`).
  * Enthält nur englische Groß-/Kleinbuchstaben (`A–Z`, `a–z`), Ziffern (`0–9`), Satzzeichen und Steuerzeichen (**keine deutschen Umlaute `ä, ö, ü, ß` und kein `€`-Zeichen!**).
  * *Wichtige ASCII-Codes für Prüfungsaufgaben:* `'0'` = Dezimal `48` (`0x30`), **`'A'` = Dezimal `65` (`0x41`)**, **`'a'` = Dezimal `97` (`0x61`)** *(Kleinbuchstaben liegen exakt $+32$ über den Großbuchstaben!)*.
* **ANSI / ISO-8859-1 (Latin-1) / ISO-8859-15:**
  * Nutzt **8 Bit (1 Byte) pro Zeichen** $\rightarrow 2^8 = \mathbf{256\text{ Zeichen}}$. Enthält in der oberen Hälfte (`128–255`) westeuropäische Umlaute (`ä, ö, ü`), kann aber nicht gleichzeitig z. B. griechische, kyrillische oder asiatische Schriftzeichen darstellen.
* **Unicode & UTF-8 (*8-Bit Unicode Transformation Format* – Weltweiter Standard im Web!):**
  * Umfasst über $149.000$ Zeichen aller Sprachen der Welt sowie Emojis.
  * **UTF-8 speichert mit variabler Länge (1 bis 4 Byte pro Zeichen):**
    * Alle klassischen **ASCII-Zeichen (`A–Z`, `0–9`) belegen nur 1 Byte** (UTF-8 ist zu $100\,\%$ abwärtskompatibel zu ASCII!),
    * Deutsche **Umlaute (`ä, ö, ü, ß`) belegen 2 Byte**,
    * Das **`€`-Zeichen** und asiatische Zeichen belegen **3 Byte**, Emojis **4 Byte**.

---

### 3. Softwarequalität & Testverfahren (Black-Box vs. White-Box & Code Coverage)

| Kriterium | Black-Box-Test (*Funktionaler Test*) | White-Box-Test (*Strukturtest*) |
| :--- | :--- | :--- |
| **Kennt der Tester den Quellcode?** | **NEIN!** Das Programm wird als undurchsichtige „schwarze Box“ betrachtet. | **JA!** Der Tester schaut direkt in den Quellcode / das Struktogramm hinein. |
| **Grundlage der Testfälle** | Fachliche Anforderungen aus dem **Lasten-/Pflichtenheft** (Eingabe $\rightarrow$ erwartete Ausgabe). | Die internen **Verzweigungen (`if/else`) und Schleifen** des Quellcodes. |
| **Typische Methoden** | **Äquivalenzklassenbildung & Grenzwertanalyse:** Eingabebereich in gültige und ungültige Klassen teilen und exakt die Ränder testen (z. B. bei „Rabatt ab 18 Jahren“ testet man `17`, `18`, `19` sowie `-1` und `150`). | **Code Coverage (Überdeckungstests):**<br>* **$C_0$ (Anweisungsüberdeckung):** Jede Codezeile mind. 1× ausgeführt.<br>* **$C_1$ (Zweigüberdeckung):** Jeder `if`- **UND** jeder `else`-Zweig (wahr & falsch) wurde mind. 1× durchlaufen! |
