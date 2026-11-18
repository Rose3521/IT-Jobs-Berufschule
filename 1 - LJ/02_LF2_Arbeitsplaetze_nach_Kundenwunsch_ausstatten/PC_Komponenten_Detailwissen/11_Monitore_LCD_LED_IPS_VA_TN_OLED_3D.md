# 🖥️ LS 1 – Thema 11: Monitore (LCD, LED-Backlight, IPS/VA/TN, OLED & 3D-Technik)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] Technische Funktionsprinzipien: **LCD-, LED-, OLED- und 3D-Monitore**
- [ ] Panel-Typen bei LCD: **TN vs. IPS vs. VA**
- [ ] Vor- und Nachteile der jeweiligen Monitor-Technologien

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. LCD- & „LED-Monitore“ (Flüssigkristalle + Hintergrundbeleuchtung)
* **Wichtige Klarstellung:** Ein im Handel als **„LED-Monitor“** verkaufter Bildschirm ist technisch ein **LCD-Monitor (*Liquid Crystal Display*), dessen Hintergrundbeleuchtung aus weißen LEDs besteht** (statt wie früher aus Leuchtstoffröhren / CCFL)!
* **Aufbau & Funktionsprinzip eines LCD/LED-Monitors (Schichten von hinten nach vorne):**
  1. **LED-Backlight (Hintergrundbeleuchtung):** Leuchtet dauerhaft flächig weiß, da Flüssigkristalle **nicht selbst leuchten** können!
  2. **1. Polarisationsfilter (z. B. vertikal):** Lässt nur vertikal schwingende Lichtwellen durch.
  3. **TFT-Transistormatrix & Flüssigkristall-Schicht (*Liquid Crystals*):** Jedes Pixel besteht aus **3 Subpixeln (Rot, Grün, Blau)**. Je nach angelegter elektrischer Spannung drehen die Flüssigkristalle die Schwingungsebene des Lichts stufenlos zwischen $0^\circ$ und $90^\circ$.
  4. **Farbfilter (R, G, B) & 2. Polarisationsfilter (horizontal, um $90^\circ$ gedreht):** Je nach Drehung der Flüssigkristalle kommt am zweiten Filter entweder gar kein Licht (Schwarz), teilweises Licht (Zwischenstufe) oder volles Licht (100 % Helligkeit) durch!

```text
AUFBAU EINES LCD/LED-PIXELS:
[ LED-Backlight (Weiß) ] ---> || 1. Polfilter || ---> [ Flüssigkristalle (drehen Licht per Spannung) ] ---> [ Farbfilter R/G/B ] ---> == 2. Polfilter == ---> Auge
```

* **Die 3 LCD-Panel-Varianten im Vergleich:**
  * **TN (*Twisted Nematic*):** Günstig, sehr schnelle Schaltzeiten für E-Sport, aber **sehr schlechte Blickwinkelstabilität** (schaut man von der Seite oder von unten drauf, invertieren die Farben sofort) und blasse Farben.
  * **IPS (*In-Plane Switching*):** Flüssigkristalle rotieren parallel zur Glasfläche $\rightarrow$ **hervorragende Blickwinkelstabilität ($178^\circ$)** und **höchste Farbtreue** (Standard für gute Office-, CAD- und Bildbearbeitungs-Monitore). *Kleiner Nachteil:* „IPS-Glow“ (Schwarz wirkt im dunklen Raum eher dunkelgrau).
  * **VA (*Vertical Alignment*):** Kristalle stehen im Ruhezustand senkrecht und blockieren das Backlight sehr gut $\rightarrow$ **bester Kontrast / Schwarzwert unter den LCDs** ($3.000:1$), aber langsamere Reaktionszeiten bei dunklen Pixelwechseln (*Schlierenbildung / Black Smearing*).

---

### 2. OLED-Monitore (*Organic Light Emitting Diode*)
* **Funktionsprinzip:**
  * Ein OLED-Display benötigt **weder eine Hintergrundbeleuchtung noch Flüssigkristalle oder Polarisationsfilter**!
  * Jedes einzelne Subpixel besteht aus **organischen Halbleiter-Schichten**, die **selbstständig leuchten**, sobald ein elektrischer Strom durch sie fließt (*Elektrolumineszenz*).
* **Vorteile von OLED gegenüber LCD/LED:**
  1. **Perfektes Schwarz & unendlicher Kontrast ($\infty : 1$):** Um Schwarz darzustellen, wird das betreffende Pixel einfach komplett stromlos geschaltet – es leuchtet überhaupt nicht! (Kein *Backlight-Bleeding* oder grauer Schleier wie bei LCD).
  2. **Ultraschnelle Reaktionszeit ($\approx 0{,}03\text{ ms}$):** Fast 100-mal schneller als LCD-Kristalle $\rightarrow$ gestochen scharfe Bewegtbilder ohne Schlieren.
  3. **Extrem dünne Bauweise** und exzellente Blickwinkel.
* **Nachteile von OLED (Wichtig für Büro-Arbeitsplätze!):**
  1. **Einbrenngefahr (*Burn-In*):** Da die organischen Leuchtstoffe mit den Betriebsstunden langsam an Leuchtkraft verlieren, nutzen sich statische Bildelemente (z. B. Windows-Taskleiste, Excel-Gitternetzlinien, Senderlogos), die stundenlang an derselben Stelle leuchten, schneller ab und bleiben als dauerhafte Schatten sichtbar!
  2. **Geringere Vollbild-Spitzenhelligkeit (*ABL – Auto Brightness Limiter*)** bei komplett weißen Büro-Dokumenten und höherer Anschaffungspreis.

---

### 3. 3D-Monitore (Stereoskopisches Sehen)
Damit das Gehirn ein räumliches 3D-Bild berechnet, müssen das **linke und das rechte Auge zwei aus leicht unterschiedlichem Winkel aufgenommene Bilder** getrennt voneinander sehen:
* **Aktives 3D (*Shutter-Technik*):** Der Monitor zeigt mit mindestens $120\text{ Hz}$ abwechselnd das linke und das rechte Bild (je $60\text{ Hz}$). Eine batteriebetriebene **Shutter-Brille** verdunkelt synchron per Funk/Infrarot abwechselnd das linke und das rechte Brillenglas. *(Vorteil: Volle Auflösung pro Auge; Nachteil: Schwere Brille mit Akku, leichtes Flimmern, dunkleres Bild).*
* **Passives 3D (*Polarisations-Technik*):** Der Monitor versieht alle geraden Pixelzeilen mit linksdrehend und alle ungeraden Zeilen mit rechtsdrehend polarisiertem Licht. Eine leichte, stromlose **Polfilterbrille** (wie im Kino) lässt pro Auge nur die passenden Zeilen durch. *(Vorteil: Günstige, leichte Brille, flimmerfrei; Nachteil: Halbiert die vertikale Bildschirmauflösung pro Auge!).*

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Warum kann ein klassischer LCD/LED-Monitor im völlig abgedunkelten Raum niemals ein absolut tiefes Schwarz darstellen, ein OLED-Monitor dagegen schon?“*
   * **Lösung:** Beim LCD/LED-Monitor leuchtet die weiße **LED-Hintergrundbeleuchtung** permanent mit voller Kraft hinter dem gesamten Bildschirm. Um Schwarz darzustellen, versuchen die Flüssigkristalle und der Polarisationsfilter das Licht nur „zuzuhalten“ – dabei dringt aber immer ein kleiner Restbetrag an Streulicht durch die Ritzen (*Backlight Bleeding / IPS-Glow*). Beim **OLED-Monitor** leuchtet jedes Pixel selbst und wird für Schwarz einfach komplett abgeschaltet ($0\text{ V}$ = völlige Dunkelheit).
2. **Frage 2:** *„Ein Kunde möchte für seine Buchhaltungsabteilung 20 Monitore kaufen, auf denen täglich 8 Stunden lang dasselbe Buchhaltungsprogramm angezeigt wird. Warum raten Sie ihm dringend von teuren OLED-Monitoren ab und empfehlen stattdessen IPS-Monitore?“*
   * **Lösung:** Bei 8 Stunden täglich statischen Menü- und Tabellenleisten würden die organischen Leuchtdioden eines OLED-Monitors ungleichmäßig altern und das Programmfenster würde sich schon nach wenigen Monaten dauerhaft in das Display **einbrennen (*Burn-In*)**. Ein **IPS-Monitor (LCD)** hat keinerlei Einbrennrisiko, ist bei weißen Tabellen heller, deutlich günstiger in der Anschaffung und bietet perfekte Blickwinkel und Farbdarstellung für den Büroalltag.
