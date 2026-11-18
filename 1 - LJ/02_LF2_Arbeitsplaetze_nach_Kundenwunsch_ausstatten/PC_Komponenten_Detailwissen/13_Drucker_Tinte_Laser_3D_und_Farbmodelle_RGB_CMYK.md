# 🖨️ LS 1 – Thema 13: Drucker (Tintenstrahl, Laserdrucker, 3D-Drucker & Farbmodelle RGB vs. CMYK)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] **Farbmodelle:** **RGB (additiv)** vs. **CMYK (subtraktiv)** technisch begründen
- [ ] **Tintenstrahldrucker** (*Bubble-Jet* vs. *Piezo*) & **Laserdrucker** (6 Prozessschritte)
- [ ] **3D-Drucker:** Technik (FDM vs. SLA), Materialien (PLA, ABS, PETG, Resin) & Kostenvergleich

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Farbmodelle im Vergleich: RGB vs. CMYK (Wichtiger KA-Schwerpunkt!)

Warum sieht ein leuchtendes Foto auf dem Monitor nach dem Ausdrucken auf Papier oft etwas dunkler oder anders aus? Weil Monitor und Drucker zwei gegensätzliche physikalische Farbmischprinzipien nutzen!

| Merkmal | RGB (**R**ed, **G**reen, **B**lue) | CMYK (**C**yan, **M**agenta, **Y**ellow, **K**ey = Black) |
| :--- | :--- | :--- |
| **Physikalisches Prinzip** | **Additive Farbmischung (Selbstleuchtende Lichtquellen!)** | **Subtraktive Farbmischung (Lichtabsorbierende Farbpigmente auf Papier!)** |
| **Startpunkt (0 % Farbe)** | **Schwarz** (ein ausgeschalteter Bildschirm ist dunkel). Jede zugeschaltete Lichtfarbe *addiert* Helligkeit hinzu! | **Weiß** (ein unbedrucktes Blatt Papier reflektiert 100 % des weißen Raumlichts). Jede gedruckte Farbschicht *zieht* (*subtrahiert* / verschluckt) bestimmte Wellenlängen ab! |
| **Ergebnis bei 100 % aller Grundfarben** | $\text{Rot} + \text{Grün} + \text{Blau} =$ **Weiß!** | $\text{Cyan} + \text{Magenta} + \text{Gelb} =$ **Dunkelbraun / Schmutziggrau!** |
| **Warum braucht CMYK ein extra „K“ (Schwarz)?** | Nicht nötig (Schwarz = einfach alle LEDs ausschalten). | 1. Weil C+M+Y zusammen kein echtes Tiefschwarz ergeben.<br>2. Weil 300 % Farbauftrag für normalen schwarzen Text das Papier völlig durchweichen und extrem viel teure Buntfarbe verschwenden würde! |
| **Typische Geräte** | **Monitore, TVs, Smartphones, Beamer, Scanner, Kameras** | **Tintenstrahldrucker, Laserdrucker, professioneller Offsetdruck** |

---

### 2. Tintenstrahldrucker vs. Laserdrucker

#### A. Tintenstrahldrucker (*Inkjet* – Flüssigtinte)
* **Thermisches Verfahren (*Bubble-Jet*, z. B. HP, Canon):** Ein winziger Heizwiderstand in jeder Düse erhitzt die Tinte in Mikrosekunden auf ca. $300\text{ °C}$. Es bildet sich eine **Dampfblase (*Bubble*)**, die explosionsartig einen Tintentropfen auf das Papier schleudert.
* **Piezoelektrisches Verfahren (z. B. Epson):** Hinter der Düse sitzt ein **Piezo-Kristall**, der sich bei Anlegen einer elektrischen Spannung schlagartig mechanisch ausdehnt und den Tropfen **ohne Hitze** herausdrückt (*Vorteil:* langlebiger Permanent-Druckkopf, präzise Tropfengröße).
* *Vor-/Nachteile Tinte:* Günstige Anschaffung, exzellenter Fotodruck auf Fotopapier, geringer Stromverbrauch im Druckbetrieb – **aber:** Düsen trocknen bei längerer Nichtbenutzung ein, teure Patronen (außer bei EcoTank-Systemen), Ausdrucke sind oft nicht wasserfest.

#### B. Laserdrucker (Elektrofotografisches Verfahren mit trockenem Tonerpulver – 6 Schritte!)
1. **Laden (*Charging*):** Die lichtempfindliche **Bildtrommel (Fotoleitertrommel)** wird im Dunkeln über eine Ladewalze auf ihrer gesamten Oberfläche gleichmäßig elektrostatisch (z. B. negativ) aufgeladen.
2. **Belichten (*Exposing*):** Ein über Drehspiegel gelenkter **Laserstrahl** (oder eine feste **LED-Zeile**) belichtet exakt die Stellen auf der Trommel, an denen später gedruckt werden soll. Durch das Licht wird die Beschichtung leitfähig und die Ladung fließt an diesen Punkten ab.
3. **Entwickeln (*Developing*):** Die Entwicklerwalze führt feines **Tonerpulver** (Kunststoffpartikel + Farbpigmente + Metalloxide) an der Trommel vorbei, das exakt an den vom Laser belichteten Stellen haften bleibt.
4. **Übertragen (*Transfer*):** Das Papier wird unter der Trommel durchgeführt und von der Rückseite durch die Transferwalze stark entgegengesetzt aufgeladen, sodass das Tonerpulver von der Trommel auf das Papier springt.
5. **Fixieren (*Fusing*):** Da das Pulver noch lose auf dem Blatt liegt, läuft das Papier durch die **Fixiereinheit (*Fuser*)**: Bei **ca. $180\text{–}200\text{ °C}$ Hitze und hohem Walzendruck** schmilzt das Kunststoff-Tonerpulver und verbindet sich untrennbar mit den Papierfasern!
6. **Reinigen (*Cleaning*):** Ein Abstreifer entfernt Toner-Reste und eine Entladelampe neutralisiert die Trommel für die nächste Seite.
* *Vor-/Nachteile Laser:* **Toner trocknet niemals ein**, extrem schnelles Drucken großer Textmengen, dokumentenecht/wasserfest, sehr **niedrige Seitenkosten (TCO)** – **aber:** höherer Anschaffungspreis, hoher Spitzenstromverbrauch beim Aufheizen der Fixiereinheit, Feinstaubemission.

---

### 3. 3D-Drucker (Additive Fertigung)

| Verfahren | Funktionsweise | Materialien & Eigenschaften | Kosten & Einsatzgebiet |
| :--- | :--- | :--- | :--- |
| **FDM / FFF** (*Fused Deposition Modeling*) | Ein Kunststoffdraht (**Filament**, $\varnothing\,1{,}75\text{ mm}$) wird von der Rolle durch eine heiße Düse (**Extruder**, $200\text{–}260\text{ °C}$) geschmolzen und Schicht für Schicht auf das Heizbett aufgetragen. | **PLA:** Aus Maisstärke, leicht druckbar, geruchsarm, aber schon ab $55\text{ °C}$ weich.<br>**PETG:** Zäh, wetterfest, guter Allrounder.<br>**ABS:** Sehr stabil & hitzebeständig (wie Lego), braucht aber geschlossenen Bauraum gegen Verzug (*Warping*) und stinkt beim Drucken. | Günstige Geräte (ab $200\text{ €}$) & günstiges Material ($\approx 15\text{–}25\text{ €/kg}$); ideal für Gehäuse, Ersatzteile, funktionale Prototypen. |
| **SLA / MSLA** (*Stereolithografie* / Resin) | Eine Bauplattform taucht in eine Wanne mit flüssigem, lichtempfindlichem **Kunstharz (*Resin*)**, das von unten durch ein UV-Display oder einen UV-Laser Schicht für Schicht ausgehärtet wird. | **Photopolymer-Harz (Resin):** Flüssig und im Rohzustand gesundheitsschädlich/reizend (Handschuhe + Waschen in Isopropanol + Nachhärten unter UV-Licht nötig!). | Extrem feine Oberfläche ohne sichtbare Schichtrillen; ideal für Dentaltechnik, Schmuck, hochdetaillierte Miniaturen. |

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Warum sollte man einen großen Büro-Laserdrucker niemals an den Batterie-Ausgang einer kleinen Arbeitsplatz-USV (Unterbrechungsfreie Stromversorgung) anschließen?“*
   * **Lösung:** Die **Fixiereinheit (*Fuser*)** eines Laserdruckers muss innerhalb weniger Sekunden auf ca. **$200\text{ °C}$ aufgeheizt** werden. Dabei zieht das Heizelement kurzzeitig einen enormen **Einschalt-/Spitzenstrom von $800\text{ bis }>1.500\text{ Watt}$**! Das würde eine normale PC-USV sofort wegen Überlast (**OPP**) zum Abschalten bringen und den Akku in kürzester Zeit leersaugen.
2. **Frage 2:** *„Bei einem Papierstau im Laserdrucker zieht ein Mitarbeiter das halb bedruckte Blatt noch VOR der hinteren Fixiereinheit aus dem Gerät. Als er mit dem Daumen über die Buchstaben streicht, hat er die gesamte schwarze Schrift am Finger. Warum?“*
   * **Lösung:** Vor der Fixiereinheit haftet das trockene **Tonerpulver** lediglich durch schwache **elektrostatische Anziehung** lose auf dem Papier. Erst durch die **$200\text{ °C}$ Hitze und den Walzendruck in der Fixiereinheit** schmilzt der Kunststoffanteil im Toner und verbindet sich fest mit dem Papier!
