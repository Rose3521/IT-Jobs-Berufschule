# 📐 Technische Formelsammlung für die AP1 (Hardware, Multimedia, Netzwerk, Strom & SLA)

> **Goldene Prüfungsregel:** Schreibe in der AP1 und in Klassenarbeiten **immer den vollständigen Rechenweg UND die Einheiten** hin!

---

## 1. Datenmengen: Dezimal- vs. Binärpräfixe & Bit vs. Byte

* Kleines **`b` (Bit)** $\leftrightarrow$ Großes **`B` (Byte)**: $\mathbf{1\text{ Byte} = 8\text{ Bit}}$ (bzw. $1\text{ Oktett}$)
* **Netzwerkgeschwindigkeiten** sind fast immer in **Bit pro Sekunde** ($\text{Mbit/s}$, $\text{Gbit/s}$) angegeben!
* **Dateigrößen & Speicher** sind fast immer in **Byte** ($\text{MB}$, $\text{MiB}$, $\text{GB}$, $\text{GiB}$) angegeben!

| Dezimalpräfix (SI – Faktor $1.000 = 10^3$) | Wert in Byte | Binärpräfix (IEC – Faktor $1.024 = 2^{10}$) | Wert in Byte |
| :--- | :--- | :--- | :--- |
| $1\text{ kB}$ (Kilobyte) | $10^3 = 1.000\text{ B}$ | $1\text{ KiB}$ (Kibibyte) | $2^{10} = 1.024\text{ B}$ |
| $1\text{ MB}$ (Megabyte) | $10^6 = 1.000^2\text{ B}$ | $1\text{ MiB}$ (Mebibyte) | $2^{20} = 1.024^2 = 1.048.576\text{ B}$ |
| $1\text{ GB}$ (Gigabyte) | $10^9 = 1.000^3\text{ B}$ | $1\text{ GiB}$ (Gibibyte) | $2^{30} = 1.024^3\text{ B}$ |
| $1\text{ TB}$ (Terabyte) | $10^{12} = 1.000^4\text{ B}$ | $1\text{ TiB}$ (Tebibyte) | $2^{40} = 1.024^4\text{ B}$ |

### Zweierpotenzen-Spickzettel ($2^0$ bis $2^{16}$)
| $2^n$ | $2^0$ | $2^1$ | $2^2$ | $2^3$ | $2^4$ | $2^5$ | $2^6$ | $2^7$ | $2^8$ | $2^9$ | $2^{10}$ | $2^{11}$ | $2^{12}$ | $2^{16}$ |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Wert** | **$1$** | **$2$** | **$4$** | **$8$** | **$16$** | **$32$** | **$64$** | **$128$** | **$256$** | **$512$** | **$1.024$** | **$2.048$** | **$4.096$** | **$65.536$** |

---

## 2. Speicherbedarf für Bilder, Video & Audio berechnen (AP1: 29 Punkte!)

### A. Unkomprimiertes Bild (Bitmap / Scan)
$$\text{Dateigröße (in Byte)} = \frac{\text{Breite (Pixel)} \times \text{Höhe (Pixel)} \times \text{Farbtiefe (Bit pro Pixel)}}{8}$$
* **Bei Scanner-Aufgaben mit DPI (*Dots per Inch*, $1\text{ Zoll} = 2{,}54\text{ cm}$):**
  $$\text{Pixelbreite} = \text{Breite in Zoll} \times \text{dpi} \qquad \text{Pixelhöhe} = \text{Höhe in Zoll} \times \text{dpi}$$
* **Typische Farbtiefen:** Schwarz-Weiß $= 1\text{ Bit}$ | Graustufen $= 8\text{ Bit}$ ($1\text{ Byte}$) | True Color RGB $= \mathbf{24\text{ Bit}}$ ($3\text{ Byte}$) | RGBA $= 32\text{ Bit}$ ($4\text{ Byte}$).

### B. Unkomprimiertes Video
$$\text{Datenmenge} = \text{Bildgröße pro Frame} \times \text{Bildwiederholrate (fps)} \times \text{Dauer in Sekunden}$$

### C. Unkomprimiertes Audio (PCM / WAV)
$$\text{Datenmenge (in Byte)} = \frac{\text{Abtastrate (Hz)} \times \text{Bittiefe (Bit)} \times \text{Kanäle (Mono=1, Stereo=2)} \times \text{Zeit (s)}}{8}$$

---

## 3. Datenübertragungszeit & Bandbreite berechnen (AP1: 10 Punkte!)

$$\text{Übertragungszeit } t\text{ (in Sekunden)} = \frac{\text{Datenmenge } D\text{ (in Bit!)}}{\text{Netto-Übertragungsrate } v\text{ (in Bit/s!)}}$$

* **Musterbeispiel ($50\text{ GiB}$ über $100\text{ Mbit/s}$ Leitung mit $20\,\%$ Protokoll-Overhead $\rightarrow 80\,\%$ Nutzdatenrate):**
  1. $v_{\text{netto}} = 100\text{ Mbit/s} \times 0{,}80 = 80\text{ Mbit/s} = 80 \cdot 10^6\text{ Bit/s} = 80.000.000\text{ Bit/s}$
  2. $D = 50\text{ GiB} \times 1.024^3\text{ Byte/GiB} \times 8\text{ Bit/Byte} = 429.496.729.600\text{ Bit}$
  3. $t = \frac{429.496.729.600\text{ Bit}}{80.000.000\text{ Bit/s}} = 5.368{,}71\text{ s} \div 60 = \mathbf{89{,}48\text{ Minuten}}$ ($1\text{ Std. } 29\text{ Min. } 29\text{ Sek.}$)

---

## 4. Stromverbrauch, Netzteil-Wirkungsgrad & USV-Dimensionierung (AP1: 28 Punkte!)

* **Wirkleistung ($P$) vs. Scheinleistung ($S$ in $\text{VA}$):**
  $$P\text{ (in W)} = S\text{ (in VA)} \times \cos \varphi \qquad \Longleftrightarrow \qquad S\text{ (in VA)} = \frac{P\text{ (in W)}}{\cos \varphi}$$
* **Netzteil-Wirkungsgrad ($\eta$):**
  $$\eta = \frac{P_{\text{ab}} \text{ (Bedarf der PC-Bauteile)}}{P_{\text{zu}} \text{ (Aufnahme aus der Steckdose)}} \quad \Longleftrightarrow \quad P_{\text{zu}} = \frac{P_{\text{ab}}}{\eta}$$
* **Stromkosten pro Jahr:**
  $$\text{Energie } W\text{ (in kWh)} = \frac{P_{\text{zu}}\text{ (in W)} \times \text{Stunden/Tag} \times \text{Tage/Jahr}}{1.000} \qquad \text{Kosten} = W\text{ (in kWh)} \times \text{Preis/kWh}$$
* **USV-Überbrückungszeit:**
  $$\text{Batterie-Energie } E\text{ (in Wh)} = U\text{ (in V)} \times Q\text{ (in Ah)} \qquad \longrightarrow \qquad t\text{ (in Stunden)} = \frac{E\text{ (in Wh)} \times \eta_{\text{USV}}}{P_{\text{Last}}\text{ (in W)}}$$

---

## 5. SLA-Verfügbarkeit & Maximale Ausfallzeit (Downtime)

$$\text{Max. erlaubte Ausfallzeit} = \text{Gesamtzeit} \times \left(1 - \frac{\text{Verfügbarkeit in \%}}{100}\right)$$
* **Basis 1 Jahr ($365\text{ Tage} = 8.760\text{ Stunden} = 525.600\text{ Minuten}$):**
  * **$99{,}0\,\%$ Verfügbarkeit:** max. **$87{,}6\text{ Stunden}$** Ausfall/Jahr
  * **$99{,}9\,\%$ Verfügbarkeit:** max. **$8{,}76\text{ Stunden}$** ($8\text{ Std. } 45\text{ Min. } 36\text{ Sek.}$) Ausfall/Jahr
  * **$99{,}99\,\%$ Verfügbarkeit:** max. **$52{,}56\text{ Minuten}$** Ausfall/Jahr
