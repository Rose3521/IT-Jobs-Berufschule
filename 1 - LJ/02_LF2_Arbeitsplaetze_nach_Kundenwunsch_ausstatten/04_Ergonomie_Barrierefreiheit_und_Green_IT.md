# 🌿 LF 2 – Ergonomie am Bildschirmarbeitsplatz, Barrierefreiheit & Green IT

> **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten  
> **AP1-Statistik:** *Ergonomierichtlinien (12 P.), Nachhaltigkeit / Green IT & Energie sparen (7 P.)* sind beliebte Punktebringer in der AP1!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Ergonomierichtlinien (ArbStättV / DGUV)** für Monitor, Stuhl, Tisch, Beleuchtung und Raumklima nennen
- [ ] Maßnahmen zur **Barrierefreiheit (BITV 2.0)** in Hardware und Software erklären
- [ ] **Green IT & Nachhaltigkeit:** Maßnahmen im Lebenszyklus (Herstellung, Betrieb, Entsorgung) & Umweltsiegel zuordnen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Ergonomierichtlinien am Bildschirmarbeitsplatz (AP1 2023H, 2026F – 12 Punkte!)

In der AP1 wird oft ein Bild eines falsch eingerichteten Büroarbeitsplatzes gezeigt oder nach **6 konkreten ergonomischen Anforderungen** gemäß Arbeitsstättenverordnung (**ArbStättV**) gefragt:

1. **Monitor-Aufstellung & Blickwinkel:**
   * Die **oberste Bildschirmzeile muss sich auf oder leicht unterhalb der Augenhöhe** befinden, sodass der Kopf leicht entspannt nach unten geneigt ist (Blickwinkel ca. $15^\circ\text{–}35^\circ$ unter der Horizontalen).
   * **Sehabstand:** Mindestens **$50\text{ bis }70\text{ cm}$** (bei großen Monitoren ab 27 Zoll ca. $70\text{–}90\text{ cm}$).
   * **Display-Eigenschaften:** Mattes / entspiegeltes Panel, flimmerfrei, frei dreh-, neig- und höhenverstellbar.
2. **Stellung zum Fenster & Beleuchtung:**
   * **Blickrichtung parallel zur Fensterfront!** *(Sitzt man mit dem Gesicht zum Fenster, wird man durch das helle Tageslicht **geblendet**; sitzt man mit dem Rücken zum Fenster, spiegelt sich das Fenster als **Reflexion auf dem Bildschirm**!).*
   * Fenster müssen mit verstellbaren Jalousien/Lamellenvorhängen gegen direkte Sonneneinstrahlung ausgestattet sein.
   * **Beleuchtungsstärke:** Mindestens **$500\text{ Lux}$** am Büroarbeitsplatz (indirektes, blendfreies Tageslichtweiß).
3. **Schreibtisch & Bürodrehstuhl:**
   * **Schreibtisch:** Höhenverstellbar (Standard-Sitzhöhe ca. $68\text{–}76\text{ cm}$, idealerweise **Sitz-Steh-Schreibtisch** für dynamischen Haltungswechsel), matte/blendfreie Oberfläche, mindestens $160 \times 80\text{ cm}$ Fläche, ausreichende Beinfreiheit.
   * **Bürostuhl:** Höhenverstellbar, anatomisch geformte Rückenlehne mit **Lordosenstütze** (Lendenwirbelstütze), **Synchronmechanik** (Sitzfläche und Lehne bewegen sich beim Zurücklehnen synchron mit), verstellbare Armlehnen, **kippsicheres Fußkreuz mit 5 lastabhängig gebremsten Rollen**.
   * **90-Grad-Regel:** Ober- und Unterarme sowie Ober- und Unterschenkel bilden jeweils einen Winkel von **ca. $90^\circ$ (oder leicht größer)**; Füße stehen flach auf dem Boden (ggf. Fußstütze nutzen).
4. **Tastatur & Maus:**
   * Tastatur muss **vom Bildschirm getrennt**, neigbar und matt sein; vor der Tastatur müssen **$10\text{–}15\text{ cm}$ freie Tischfläche als Handballenauflage** bleiben.
   * Bei Handgelenksbeschwerden (**RSI-Syndrom / Mausarm / Karpaltunnelsyndrom**): **Vertikalmaus** (natürliche Handhaltung wie beim Händeschütteln) und geteilte Ergonomie-Tastatur einsetzen.
5. **Raumklima & Lärmschutz:**
   * Raumtemperatur ca. **$20\text{–}22\text{ °C}$** (im Sommer max. $26\text{ °C}$), relative Luftfeuchtigkeit **$40\text{–}60\,\%$**.
   * **Geräuschpegel:** Bei geistiger/konzentrierter Arbeit maximal **$55\text{ dB(A)}$** (bzw. $35\text{–}45\text{ dB(A)}$; laute Server und große Abteilungs-Laserdrucker gehören in separate Räume!).

---

### 2. Barrierefreiheit (BITV 2.0 / Barrierefreie-Informationstechnik-Verordnung)
* **Hardware für körperliche Einschränkungen:**
  * *Sehbehinderung / Blindheit:* **Braillezeile** (ertastbare Blindenschrift-Zeile), große kontrastreiche Monitore, Vorlesegeräte.
  * *Motorische Einschränkungen:* Sprachsteuerung, Augensteuerung (*Eye-Tracking*), Trackball, spezielle Großfeldtastaturen.
* **Software & Webseiten (4 Grundprinzipien nach WCAG: Wahrnehmbar, Bedienbar, Verständlich, Robust):**
  * Komplette **Tastaturbedienbarkeit** ohne Maus (Tab-Reihenfolge!),
  * Kompatibilität mit **Screenreadern** durch **Alternativtexte (`alt`-Attribut)** für alle Bilder und saubere semantische Überschriftenstruktur (`<h1>`, `<h2>`),
  * Hohes **Kontrastverhältnis** und Verzicht auf reine Farb-Codierung (wichtig bei Rot-Grün-Sehschwäche!),
  * **Untertitel** und Audiodeskription für Videos, **Leichte Sprache**.

---

### 3. Green IT & Nachhaltigkeit (AP1 2021H, 2024H)
* **Maßnahmen im laufenden IT-Betrieb:**
  * Einsatz energieeffizienter Hardware (**Thin Clients** statt schwerer Desktop-PCs, **SSDs** statt HDDs, **80-PLUS-Gold/Titanium-Netzteile**),
  * **Server-Konsolidierung durch Virtualisierung:** Statt 10 physischer Server mit je $15\,\%$ Auslastung läuft nur noch 1 physischer Server mit 10 VMs bei $80\,\%$ Auslastung!
  * Automatische **Energiesparpläne per Gruppenrichtlinie (GPO)** (Monitor nach 5 Min., Ruhezustand nach 15 Min. Inaktivität),
  * **Kaltgang-/Warmgang-Einhausung** und Freikühlung im Rechenzentrum (niedriger **PUE-Wert** – *Power Usage Effectiveness*, ideal nahe $1{,}0$),
  * *Duplexdruck* (beidseitig) und *Follow-Me-Printing* (Druckauftrag wird erst gedruckt, wenn der Mitarbeiter seine Karte an den Drucker hält $\rightarrow$ vermeidet vergessene Papierberge!).
* **Wichtige Umweltsiegel für IT-Geräte:**
  * **Blauer Engel (Deutschland):** Strenges Umweltzeichen für geringen Energieverbrauch, Schadstofffreiheit, Langlebigkeit, Reparierbarkeit und leisen Betrieb.
  * **Energy Star (USA/International):** Fokus auf niedrigen Stromverbrauch im Betrieb und Standby.
  * **TCO Certified:** Kombiniert **ökologische Kriterien** (Energie, Schadstoffe), **Ergonomie** (Bildqualität, Strahlungsarmut) und **soziale Verantwortung (CSR)** in den Fabriken der Lieferkette!
* **Fachgerechte Entsorgung (ElektroG / WEEE-Richtlinie):**
  * Ausgemusterte IT-Geräte dürfen **niemals in den Hausmüll**, sondern müssen als Elektroschrott getrennt gesammelt und recycelt werden (**Achtung Datenschutz:** Vor Weitergabe oder Verschrottung müssen alle Datenträger zwingend nach **BSI-Vorgaben mehrfach überschrieben / kryptografisch gelöscht oder physisch geschreddert** werden!).
