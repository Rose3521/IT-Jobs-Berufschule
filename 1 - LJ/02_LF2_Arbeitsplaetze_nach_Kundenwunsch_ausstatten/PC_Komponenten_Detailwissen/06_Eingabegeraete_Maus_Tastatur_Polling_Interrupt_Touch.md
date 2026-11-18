# 🖱️ LS 1 – Thema 6: Eingabegeräte (Maus-Sensorik, Tastatur Interrupt vs. Polling & Touch-Displays)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] **Optische Maus:** Sensorik, CMOS-Kamera, DSP, DPI/CPI (keine Kugelmaus!)
- [ ] **Tastatur / Signalverarbeitung:** **Interrupt vs. Polling** im technischen Vergleich
- [ ] **Touch-Displays:** **Resistiv vs. Kapazitiv** (Aufbau, Funktionsweise, Bedienkonzepte & Multitouch-Gesten)

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Optische Maus (LED / Laser-Sensorik)
* **Funktionsprinzip (Optische Korrelation):**
  1. Eine **Leuchtdiode (LED)** oder **Infrarot-/Laserdiode** beleuchtet die Tischoberfläche unter der Maus in einem flachen Winkel, sodass selbst kleinste Unebenheiten mikroskopische Schatten werfen.
  2. Eine winzige **Hochgeschwindigkeits-Kamera (CMOS-Sensor)** mit kleiner Linse fotografiert diesen winzigen Oberflächenausschnitt **1.000- bis über 20.000-mal pro Sekunde (FPS)**.
  3. Ein integrierter **DSP (*Digital Signal Processor*)** vergleicht jedes neue Bild mit dem vorherigen Bild, erkennt anhand verschobener Texturmuster, in welche Richtung ($\Delta X, \Delta Y$) und wie weit die Maus bewegt wurde, und sendet diese Koordinatenänderung an den PC.
* **DPI / CPI (*Dots / Counts per Inch*):** Gibt an, wie viele Einzelschritte der Sensor pro zurückgelegtem Zoll ($2{,}54\text{ cm}$) Mausbewegung an den PC meldet (Büro: $800\text{–}1.600\text{ DPI}$; Gaming/CAD: bis $>25.000\text{ DPI}$).

---

### 2. Tastatur & Schnittstellen-Kommunikation: Interrupt vs. Polling (KA-Klassiker!)

Wie erfährt die CPU, dass der Benutzer gerade eine Taste auf der Tastatur gedrückt oder die Maus bewegt hat?

| Kriterium | Polling (*Zyklisches Abfragen*, z. B. bei USB!) | Interrupt (*Unterbrechungssteuerung*, z. B. PS/2, Systembus) |
| :--- | :--- | :--- |
| **Prinzip** | Der **Host-Controller / die CPU fragt das Gerät in einer Endlosschleife aktiv ab**: *„Hast du neue Eingaben?“* (Bei $1.000\text{ Hz}$ USB-Polling-Rate exakt jede $1\text{ ms}$!). | Die CPU rechnet ungestört. Erst **wenn tatsächlich eine Taste gedrückt wird**, sendet das Gerät ein elektrisches Signal über die **IRQ-Leitung (*Interrupt Request*)** an den Interrupt-Controller. Die CPU pausiert kurz ihren aktuellen Thread, führt die *Interrupt Service Routine (ISR)* aus (liest das Zeichen) und arbeitet sofort weiter. |
| **Vorteile** | Sehr einfache Protokollsteuerung durch den Host; die maximale Bus-Last ist fest kalkulierbar. | **Keine verschwendete Rechenzeit im Leerlauf** (wenn stundenlang niemand tippt, finden 0 Abfragen statt); sofortige Reaktion auf Ereignisse. |
| **Nachteile** | **Unnötiger Overhead im Leerlauf**, da auch ohne Tastendruck permanent Abfragepakete über den Bus geschickt werden; Reaktionszeit hängt vom Abfrageintervall ab. | Treten extrem viele Interrupts gleichzeitig auf (*Interrupt-Sturm*), verbringt die CPU mehr Zeit mit dem Wechseln des Kontexts als mit eigentlicher Arbeit. |

---

### 3. Touch-Displays: Resistiv vs. Kapazitiv

| Merkmal | Resistiver Touchscreen (*Widerstand / Druck*) | Kapazitiver Touchscreen (*Elektrisches Feld*) |
| :--- | :--- | :--- |
| **Technischer Aufbau** | Zwei durchsichtige, elektrisch leitfähige **ITO-Folien** liegen mit einem winzigen Luftspalt (Mikro-Abstandspunkte) übereinander. | Eine feste **Echtglasplatte** ist mit einer leitfähigen Schicht überzogen; die vier Ecken bauen ein gleichmäßiges **elektrostatisches Wechselspannungsfeld** auf. |
| **Was löst den Klick aus?** | **Mechanischer Druck!** Die obere Folie biegt sich durch und berührt die untere $\rightarrow$ der geänderte elektrische Widerstand am Kontaktpunkt verrät die $(X, Y)$-Koordinate. | **Elektrische Leitfähigkeit!** Berührt ein leitfähiger menschlicher Finger das Glas, fließt ein winziger Teil der Ladung über den Körper ab (**Kapazitätsänderung** an den Ecken wird gemessen). |
| **Womit bedienbar?** | **Mit jedem beliebigen Gegenstand:** Finger, **dicke Arbeitshandschuhe**, normaler Kugelschreiber/Plastikstift, Kreditkarte. | **Nur mit leitfähigen Objekten:** Bloßer Finger oder spezieller kapazitiver Stylus *(normale Handschuhe oder Plastikstifte funktionieren NICHT!)*. |
| **Multitouch & Gesten** | Meist nur **Single-Touch** (1 Punkt; keine 2-Finger-Zoom-Gesten möglich). | **Echtes Multitouch** (10+ Finger gleichzeitig für *Pinch-to-Zoom*, Drehen, Wischen). |
| **Haltbarkeit** | Weiche Kunststoff-Oberfläche verkratzt leicht und verschleißt mechanisch. | Extrem kratzfestes Glas, kein mechanischer Verschleiß, brillantes Bild. |
| **Typischer Einsatz** | **Industrie-Steuerungen (CNC), Kälte-/Lagerlogistik, ältere Bankautomaten, Kassen** | **Smartphones, Tablets, Notebooks, moderne Info-Terminals** |

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Auf Ihrem Smartphone ist ein dicker Wassertropfen gelandet. Plötzlich öffnet das Display wie von Geisterhand Apps oder reagiert nicht mehr richtig auf Ihren Finger. Warum passiert das bei einem kapazitiven Touchscreen, während ein resistiver Touchscreen davon völlig unbeeindruckt bleibt?“*
   * **Lösung:** Wasser (mit gelösten Mineralien/Salzen) ist **elektrisch leitfähig**! Auf einem **kapazitiven Touchscreen** verändert der Wassertropfen genau wie ein menschlicher Finger das elektrostatische Feld auf der Glasoberfläche, sodass der Controller eine Geister-Berührung (*Ghost Touch*) erkennt. Ein **resistiver Touchscreen** reagiert dagegen ausschließlich auf **mechanischen Druck** (Durchbiegen der oberen Folie) – und da ein Wassertropfen praktisch nichts wiegt, löst er dort keinen Kontakt aus.
2. **Frage 2:** *„Warum bewirbt man Gaming-Mäuse mit einer Polling-Rate von 1.000 Hz oder 8.000 Hz, und warum kann eine 8.000-Hz-Polling-Rate auf einem älteren Büro-PC sogar zu ruckelnden Spielen führen?“*
   * **Lösung:** Bei $1.000\text{ Hz}$ fragt der PC die Maus jede $1\text{ ms}$ ab, bei $8.000\text{ Hz}$ sogar alle $0{,}125\text{ ms}$ (8.000-mal pro Sekunde!). Das verringert zwar die Eingabeverzögerung (*Input Lag*) minimal, zwingt aber die CPU des PCs dazu, 8.000-mal pro Sekunde USB-Datenpakete zu verarbeiten, was auf schwächeren CPUs eine spürbare Prozessorlast erzeugt.
