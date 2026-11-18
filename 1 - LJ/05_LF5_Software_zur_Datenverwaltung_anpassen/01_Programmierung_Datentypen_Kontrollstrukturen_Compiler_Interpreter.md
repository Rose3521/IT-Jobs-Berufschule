# 💻 LF 5 – Teil 1: Grundlagen der Programmierung, Datentypen, Kontrollstrukturen & Compiler vs. Interpreter

> **Lernfeld 5:** Software zur Verwaltung von Daten anpassen (SAE / BFK-S)  
> **AP1-Statistik:** *Compiler vs. Interpreter & Programmiersprache auswählen (11 P.), Datentypen (3 P.), Code erklären & PowerShell (14 P.)* = **28 Punkte** in der AP1!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Elementare Datentypen** (`int`, `double`/`decimal`, `bool`, `char`, `string`) und **Arrays/Listen** passend auswählen
- [ ] **Operatoren** (Arithmetisch inkl. Modulo `%`, Vergleich, Logisch `AND`/`OR`/`NOT`/`XOR`) anwenden
- [ ] **Kontrollstrukturen** (Verzweigungen `if/else`, `switch/case` & die 3 Schleifentypen `while`, `do-while`, `for`) unterscheiden
- [ ] **Compiler vs. Interpreter vs. Bytecode/JIT** technisch vergleichen und eine passende Programmiersprache begründen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Elementare Datentypen & Wann man welchen wählt (AP1 Herbst 2026)

| Datentyp | Beschreibung | Speichergröße (typisch) | Wertebereich / Beispiel | Prüfungsfalle / Wann nutzen? |
| :--- | :--- | :---: | :--- | :--- |
| **`int` / `Integer`** | Ganze Zahl (mit Vorzeichen) | 4 Byte (32 Bit) | $-2{,}14\text{ Mrd. bis }+2{,}14\text{ Mrd.}$ (`42`, `-7`, `0`) | Stückzahlen, Schleifenzähler, IDs. *(Achtung: **NIEMALS** für Postleitzahlen oder Telefonnummern nutzen, da führende Nullen wie `0711` oder `01234` bei `int` weggeschnitten werden $\rightarrow$ dafür immer **`String`** nehmen!)* |
| **`double` / `float`** | Binäre Gleitkommazahl (IEEE 754) | 8 Byte / 4 Byte | `3.14159`, `-0.0025` | Wissenschaftliche Berechnungen, Messwerte, Grafik. *(Achtung: Binäre Rundungsungenauigkeiten wie `0.1 + 0.2 = 0.30000000000000004`!).* |
| **`decimal` / `Currency`** | Dezimale Fest-/Gleitkommazahl | 16 Byte (128 Bit) | `19.99`, `1450.50` | **Pflicht für Geldbeträge / Währungen / Preise in kaufmännischer Software**, da exakt im Dezimalsystem ohne Rundungsfehler gerechnet wird! |
| **`bool` / `Boolean`** | Wahrheitswert | 1 Bit (bzw. 1 Byte) | `true` (`1`) oder `false` (`0`) | Status-Flags (`istAktiv`, `hatBezahlt`), Bedingungen in `if`- und `while`-Abfragen. |
| **`char` / `Character`** | Einzelnes Schriftzeichen | 1–2 Byte | `'A'`, `'?'`, `'9'` | Einzelbuchstabe, Geschlechtskürzel (`'m'`, `'w'`, `'d'`). |
| **`string`** | Zeichenkette (Text) | Variabel | `"Max Mustermann"`, `"0711-12345"` | Namen, Adressen, **Postleitzahlen (`"01067"`)**, **Telefonnummern**, **IBAN**. |

* **Arrays (Felder):**
  * Speichern mehrere Werte **desselben Datentyps** unter einem gemeinsamen Namen.
  * **Der Index beginnt in fast allen Programmiersprachen (C#, Java, Python, C++, JS, PowerShell) bei `0`!**
  * Hat ein Array `zahlen` genau `5` Elemente (`zahlen.Length == 5`), lauten die Indizes `zahlen[0]` (erstes Element) bis `zahlen[4]` (letztes Element = `Length - 1`).

---

### 2. Wichtige Operatoren & Die 3 Schleifentypen
* **Modulo-Operator (`%` oder `MOD`):** Berechnet den **ganzzahligen Rest einer Division**!
  * `14 % 4 = 2` (denn $14 \div 4 = 3\text{ Rest }\mathbf{2}$)
  * `zahl % 2 == 0` $\rightarrow$ Prüft, ob `zahl` **gerade** ist!
  * `jahr % 4 == 0` $\rightarrow$ Prüft, ob `jahr` ohne Rest durch 4 teilbar ist (Schaltjahr-Prüfung).
* **Logische Verknüpfungen (Boolesche Algebra / Wahrheitstabelle):**
  * **`AND` (`&&` / Konjunktion):** Nur `true`, wenn **beide** Seiten `true` sind.
  * **`OR` (`||` / Disjunktion):** Immer `true`, sobald **mindestens eine** Seite `true` ist.
  * **`XOR` (`^` / Kontravalenz / Exklusiv-Oder):** Nur `true`, wenn beide Seiten **unterschiedlich** sind (genau eine Seite `true`).
  * **`NOT` (`!` / Negation):** Kehrt `true` in `false` um (und umgekehrt).

| Schleifenart | Bedingungsprüfung | Mindestdurchläufe | Typisches Einsatzszenario |
| :--- | :--- | :---: | :--- |
| **Kopfgesteuerte Schleife (`while`)** | **Vor** jedem Durchlauf (am Schleifenkopf) | **0-mal** (wird übersprungen, wenn Bedingung von Anfang an falsch ist) | Lesen aus einer Datei/Datenbank, solange noch Datensätze vorhanden sind (`while (reader.Read())`). |
| **Fußgesteuerte Schleife (`do-while`)** | **Nach** jedem Durchlauf (am Schleifenfuß) | **Mindestens 1-mal!** | Benutzereingabe / Passwortabfrage, die erst eingegeben werden muss, bevor man sie prüfen kann. |
| **Zählschleife (`for` / `foreach`)** | Vorab mit Laufvariable (`for (int i = 0; i < n; i++)`) | Bekannte Anzahl ($n$-mal) | Durchlaufen aller Elemente eines Arrays oder einer Liste. |

---

### 3. Compiler vs. Interpreter vs. Bytecode/JIT (AP1 Herbst 2024 & Frühjahr 2026!)

| Kriterium | Compiler-Sprachen (z. B. C, C++, Rust, Go) | Interpreter-Sprachen (z. B. Python, JavaScript, PHP, PowerShell) | Hybrid: Bytecode + JIT-Compiler (z. B. C# / .NET, Java) |
| :--- | :--- | :--- | :--- |
| **Übersetzungszeitpunkt** | Der gesamte Quellcode wird **vor der Auslieferung einmalig komplett** in nativen Maschinencode (`.exe`) übersetzt. | Der Quellcode wird **erst zur Laufzeit Zeile für Zeile** eingelesen, übersetzt und sofort ausgeführt. | Quellcode wird vorab in plattformunabhängigen **Bytecode / IL** übersetzt; zur Laufzeit übersetzt die **VM (CLR / JVM)** diesen per **JIT (*Just-in-Time*)** in Maschinencode. |
| **Ausführungsgeschwindigkeit** | **Maximal schnell** (läuft direkt auf der CPU ohne Zwischenschicht). | **Langsamer** (durch die ständige Übersetzung während des Programmablaufs). | **Sehr schnell** (nahe an C++, da der JIT-Compiler häufig genutzte Schleifen im RAM nativ kompiliert). |
| **Plattformunabhängigkeit** | **Gering:** Muss für jedes Betriebssystem (Windows, Linux, macOS) und jede CPU separat kompiliert werden. | **Sehr hoch:** Dasselbe Skript läuft sofort auf jedem System, auf dem der passende Interpreter installiert ist. | **Sehr hoch:** Dieselbe kompilierte `.dll`/`.jar`-Datei läuft auf jedem OS mit installierter .NET Runtime / JVM. |
| **Fehlersuche & Quellcodeschutz** | Syntaxfehler werden **alle vorab beim Kompilieren** gemeldet; der **Quellcode bleibt beim Hersteller geheim** (Kunde erhält nur die `.exe`). | Ein Tippfehler in Zeile 200 fällt erst auf, wenn das Programm zur Laufzeit tatsächlich Zeile 200 erreicht; **Quellcode liegt im Klartext vor**. | Syntax-/Typfehler werden vorab beim Kompilieren erkannt; gute Balance für Unternehmenssoftware. |
