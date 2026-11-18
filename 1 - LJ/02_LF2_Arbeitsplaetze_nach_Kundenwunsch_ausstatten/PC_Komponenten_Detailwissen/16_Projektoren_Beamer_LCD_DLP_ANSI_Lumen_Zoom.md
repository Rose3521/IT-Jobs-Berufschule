# 📽️ LS 1 – Thema 16: Projektoren / Beamer (LCD vs. DLP, ANSI-Lumen, Optischer Zoom & Kühlung)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] Projektionstechnologien: **3LCD-Technik vs. DLP-Technik**
- [ ] **Helligkeit (ANSI-Lumen)** für verschiedene Raumbedingungen
- [ ] **Optischer Zoom vs. Digitaler Zoom** & Projektionsverhältnis (*Throw Ratio*)
- [ ] **Lichtquellen (UHP-Lampe vs. Laser/LED)**, Wärmeentwicklung & Kühlung

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Projektionstechnologien im Vergleich: 3LCD vs. DLP

| Kriterium | 3LCD-Projektor (*Liquid Crystal Display*) | DLP-Projektor (*Digital Light Processing*) |
| :--- | :--- | :--- |
| **Technisches Funktionsprinzip** | Das weiße Licht der Lampe wird durch halbdurchlässige Spiegel (*dichroitische Spiegel*) in die 3 Grundfarben **Rot, Grün und Blau** aufgeteilt. Jeder Farbstrahl durchleuchtet ein **eigenes kleines LCD-Panel (insg. 3 Panels)**. Ein Prisma setzt die drei Teilbilder wieder zu einem vollfarbigen Bild zusammen und wirft es durch das Objektiv auf die Leinwand. | Das Licht fällt auf einen einzigen **DMD-Chip (*Digital Micromirror Device*)**, auf dem Millionen mikroskopisch kleine, elektrisch kippbare **Aluminium-Spiegel** sitzen (1 Kippspiegel = 1 Pixel, kippt bis zu 5.000-mal pro Sekunde zwischen „Licht ins Objektiv“ = Hell und „Licht in den Absorber“ = Schwarz). Für die Farbe rotiert davor ein **schnelles Farbrad (R, G, B, W)**, sodass die Farben **nacheinander** projiziert werden und erst im trägen menschlichen Auge verschmelzen! |
| **Vorteile** | **Sehr hohe Farbhelligkeit** (Farben wirken genauso hell wie Weiß), **kein Regenbogeneffekt**, sehr scharfe Textdarstellung. | **Sehr hoher Kontrast / besserer Schwarzwert**, gekapselter Lichtweg (unempfindlich gegen Staub), sehr kompakte Bauweise, kein Einbrennen/Ausbleichen von LCDs. |
| **Nachteile** | Offenerer Lichtweg $\rightarrow$ benötigt **Staubfilter**, die regelmäßig gereinigt werden müssen; geringerer Schwarzwert als DLP; organische Polfilter altern nach vielen Jahren. | **Regenbogeneffekt (RBE):** Durch das nacheinander rotierende Farbrad sehen empfindliche Zuschauer bei schnellen Augenbewegungen farbige Blitze an hellen Kanten! |

---

### 2. Helligkeit (ANSI-Lumen) & Optischer Zoom
* **Lichtstrom in ANSI-Lumen (Genormtes Messverfahren über 9 Messpunkte auf der Leinwand):**
  * **Abgedunkeltes Heimkino:** $1.500\text{ bis }2.200\text{ ANSI-Lumen}$ reichen völlig aus.
  * **Normales Büro / Klassenzimmer mit Tageslicht:** Mindestens **$3.500\text{ bis }4.500\text{ ANSI-Lumen}$** erforderlich, da das Umgebungslicht sonst das projizierte Bild überstrahlt!
  * **Großer Hörsaal / Messehalle:** **$6.000\text{ bis }>15.000\text{ ANSI-Lumen}$**.
* **Optischer Zoom vs. Digitaler Zoom (Wichtig bei der Festmontage an der Decke!):**
  * **Optischer Zoom (z. B. $1{,}6\times$):** Verschiebt **echte Glaslinsen** im Objektiv mechanisch gegeneinander. Dadurch lässt sich das Bild größer oder kleiner stellen, **ohne dass auch nur ein einziges Pixel an Auflösung oder Lichtstärke verloren geht!**
  * **Digitaler Zoom (und digitale Trapezkorrektur / *Keystone*):** Verkleinert das genutzte Bildfeld nur elektronisch auf dem Chip $\rightarrow$ **massiver Verlust an nativer Auflösung (Treppchenbildung bei Text!), Schärfe und Lichtleistung!** *(Im Idealfall nutzt man statt digitaler Trapezkorrektur einen **optischen Lens-Shift**, der das Objektiv mechanisch verschiebt).*

---

### 3. Lichtquelle, Wärmeentwicklung & Kühlung
* **Klassische UHP-Hochdruck-Quecksilberdampflampe:**
  * Erzeugt einen extrem hellen Lichtbogen bei **über $800\text{–}1.000\text{ °C}$** und hohem Innendruck ($\approx 200\text{ bar}$).
  * *Folgen:* Hoher Stromverbrauch ($200\text{–}350\text{ W}$), braucht ca. 1 Minute zum Aufheizen, lautes Lüftergeräusch, Lebensdauer nur **$3.000\text{–}6.000\text{ Stunden}$** (teure Ersatzlampen!).
  * **Lebenswichtige Betriebsregel:** Nach dem Ausschalten per Fernbedienung läuft der Lüfter noch 1–2 Minuten weiter (**Nachlaufkühlung**). **Zieht man im heißen Zustand einfach das Stromkabel heraus, kommt es zum Hitzestau und der Glaskolben der Lampe kann platzen!**
* **Moderne Laser-Phosphor- & LED-Beamer:**
  * Starten in **2 Sekunden** sofort mit voller Helligkeit, benötigen **keine Nachlaufkühlung**, verbrauchen deutlich weniger Strom und halten **$20.000\text{ bis }30.000\text{ Stunden}$** (kein Lampenwechsel über die gesamte Lebensdauer nötig!).

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Auf einem Beamer kann man niemals echtes ‚Schwarz‘ auf eine weiße Wand projizieren, wenn im Klassenzimmer das Deckenlicht brennt – selbst wenn der Beamer 10.000 Euro kostet. Erklären Sie physikalisch, warum das so ist!“*
   * **Lösung:** Ein Beamer kann immer nur **Licht hinzufügen** (additive Mischung), aber kein Licht „absaugen“. „Schwarz“ bei einer Projektion bedeutet lediglich, dass der Beamer an dieser Stelle **gar kein eigenes Licht** auf die Leinwand wirft. Eine Stelle auf der Leinwand kann also **niemals dunkler sein als die unbeleuchtete weiße Leinwand im aktuellen Raumlicht**! Ist das Klassenzimmer hell erleuchtet, erscheint die weiße Leinwand hellgrau/weiß – und damit ist auch das „Schwärzeste“, was der Beamer darstellen kann, nur dieses Hellgrau (Abhilfe: Raum abdunkeln oder spezielle graue Kontrastleinwand nutzen).
2. **Frage 2:** *„Warum sollte man beim Decken-Einbau eines Beamers den Abstand zur Leinwand immer so berechnen, dass man mit dem optischen Zoom auskommt, statt den Beamer irgendwo hinzuhängen und das schiefe/zu große Bild einfach per digitalem Zoom und digitaler Trapezkorrektur (Keystone) zurechtzudrücken?“*
   * **Lösung:** Bei der digitalen Trapezkorrektur und beim digitalen Zoom werden große Teile der physischen Pixel auf dem LCD-/DLP-Chip einfach **deaktiviert (schwarz geschaltet)** und das verbleibende Bild per Software kleingerechnet. Dadurch verliert man bis zu $30\text{–}40\,\%$ der echten **Auflösung** (kleine Excel-Zahlen und Quellcode werden unscharf/unleserlich), verliert entsprechend viel **Helligkeit** und sieht um das eigentliche Bild herum einen dauerhaft grauen Lichtrahmen (*Lichthof*) an der Wand!
