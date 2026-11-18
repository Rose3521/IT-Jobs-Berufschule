# 🎵 LS 1 – Thema 4: Multimediakomponenten / Soundkarte (AD/DA-Wandlung, Abtastrate & Bittiefe)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] Funktionsweise einer Soundkarte (**AD-Wandlung** & **DA-Wandlung**)
- [ ] **Abtastrate (*Sampling Rate* in Hz)** & **Nyquist-Shannon-Abtasttheorem**
- [ ] **Bittiefe (*Quantisierung* in Bit)** & Quantisierungsrauschen
- [ ] Grafisches Beispiel: **Umwandlung eines Analogsignals in ein Digitalsignal** + Speicherbedarfsberechnung

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Warum braucht man eine AD/DA-Wandlung?
* **Schallwellen** in der Luft werden von einem Mikrofon in eine **kontinuierliche, analoge elektrische Wechselspannung** umgewandelt.
* Da eine CPU und digitale Speichermedien nur mit diskreten Binärzahlen (`0` und `1`) arbeiten können, besitzt jede Soundkarte (bzw. jedes Audio-Interface / USB-Headset) zwei zentrale Wandler-Chips:
  1. **ADC (*Analog-to-Digital Converter* / AD-Wandler):** Wandelt das analoge Mikrofon-/Line-In-Signal in digitale Binärdaten um (**Aufnahme**).
  2. **DAC (*Digital-to-Analog Converter* / DA-Wandler):** Wandelt digitale Audiodaten (MP3, WAV) wieder in eine analoge Wechselspannung für Kopfhörer oder Lautsprecher um (**Wiedergabe**).

---

### 2. Die 3 Schritte der AD-Wandlung (PCM – Pulscodemodulation)

1. **Abtasten (*Sampling* – Zeitachse / X-Achse):**
   * Das analoge Signal wird in festen, exakt gleichen Zeitabständen gemessen.
   * **Abtastrate (*Sampling Rate* in $\text{Hz}$):** Anzahl der Messungen pro Sekunde (z. B. Audio-CD: **$44{,}1\text{ kHz} = 44.100\text{ Messungen/s}$**; Video-Tonspur: **$48\text{ kHz}$**).
   * **Nyquist-Shannon-Abtasttheorem:** Um eine maximale Tonfrequenz $f_{\max}$ ohne Informationsverlust (und ohne *Aliasing*-Störtöne) digitalisieren zu können, muss die Abtastrate **mindestens doppelt so hoch** sein wie die höchste vorkommende Signalfrequenz:
     $$f_{\text{Abtast}} > 2 \cdot f_{\max}$$
     *(Menschliches Gehör hört bis ca. $20\text{ kHz} \rightarrow$ mind. $2 \times 20\text{ kHz} = 40\text{ kHz}$ nötig $\rightarrow$ daher nutzt die CD **$44{,}1\text{ kHz}$**!)*
2. **Quantisieren (Wertachse / Y-Achse – Bittiefe):**
   * Da ein Analogwert unendlich viele Nachkommastellen haben kann, muss jeder Messwert auf den nächstgelegenen digitalen Stufenwert **gerundet** werden.
   * **Bittiefe (*Auflösung* in Bit):** Legt fest, wie viele Stufen ($2^n$) auf der Y-Achse existieren:
     * **$8\text{ Bit}$:** $2^8 = 256$ Stufen (grobe Rundung $\rightarrow$ hörbares **Quantisierungsrauschen**)
     * **$16\text{ Bit}$ (CD-Standard):** $2^{16} = \mathbf{65.536\text{ Stufen}}$ (sehr feine Auflösung)
     * **$24\text{ Bit}$ (Tonstudio):** $2^{24} = \mathbf{16.777.216\text{ Stufen}}$ (maximaler Dynamikumfang)
3. **Codieren:**
   * Jede gemessene Stufe wird als Binärzahl gespeichert:

```text
BEISPIEL: UMWANDLUNG EINES ANALOGSIGNALS IN EIN DIGITALSIGNAL (3 Bit Bittiefe = 8 Stufen)

Amplitude / Spannung (Quantisierungsstufen 000 bis 111)
111 (7) |          *---*
110 (6) |        *       *
101 (5) |      *           *
100 (4) |    *               *
011 (3) |  *                   *               *
010 (2) |*                       *           *
001 (1) |                          *       *
000 (0) +----------------------------*---*------> Zeit (Abtastzeitpunkte t1..t9)
         t1  t2  t3  t4  t5  t6  t7  t8  t9
Stufe:    2   4   6   7   5   2   0   1   3
Binär:  010 100 110 111 101 010 000 001 011  --> Gespeicherter Bitstrom!
```

---

### 3. Formel: Unkomprimierte Audio-Dateigröße berechnen
$$\text{Datenmenge (in Byte)} = \frac{\text{Abtastrate (Hz)} \times \text{Bittiefe (Bit)} \times \text{Kanäle (Mono=1, Stereo=2)} \times \text{Zeit (s)}}{8}$$
* *Beispiel (1 Minute Musik in CD-Qualität: $44.100\text{ Hz}$, $16\text{ Bit}$, Stereo $= 2$ Kanäle, $60\text{ s}$):*
  $$\text{Größe} = \frac{44.100 \times 16 \times 2 \times 60}{8} = 10.584.000\text{ Byte} \approx \mathbf{10{,}58\text{ MB}} \quad (\text{bzw. } 10{,}09\text{ MiB})$$

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Warum entsteht bei der AD-Wandlung selbst mit modernster Technik immer ein kleiner Informationsverlust gegenüber dem echten analogen Ursprungssignal, und durch welche zwei Maßnahmen lässt sich dieser Verlust minimieren?“*
   * **Lösung:** Weil ein kontinuierliches Signal (unendlich viele Zeitpunkte und unendlich feine Spannungswerte) in ein endliches Raster gepresst wird (*Treppenstufen-Effekt*): Zwischen zwei Abtastzeitpunkten gehen Informationen auf der Zeitachse verloren, und beim Runden auf die nächste Stufe entsteht auf der Wertachse der **Quantisierungsfehler (Quantisierungsrauschen)**. Minimieren lässt sich das durch **1. eine höhere Abtastrate** (engere Abstände auf der X-Achse) und **2. eine höhere Bittiefe** (feinere Stufen auf der Y-Achse) – auf Kosten einer größeren Datei.
2. **Frage 2:** *„Ein Musiker schließt seine E-Gitarre über den Onboard-Soundchip direkt neben der Hochleistungs-Grafikkarte an und hört im Kopfhörer ein leises Fiepen und Brummen, sobald er die Maus bewegt oder ein 3D-Spiel startet. Warum passiert das und warum löst ein externes USB-Audio-Interface dieses Problem sofort?“*
   * **Lösung:** Auf dem Mainboard herrschen starke elektromagnetische Störfelder durch die hochfrequenten Ströme von CPU, Spannungswandlern und Grafikkarte. Diese streuen in die **analogen** Leiterbahnen des Onboard-Soundchips vor dem Ausgang ein. Bei einem **externen USB-Audio-Interface** werden die Audiodaten noch **rein digital und störsicher (symmetrisch)** per USB aus dem PC-Gehäuse herausgeführt; die empfindliche **DA-Wandlung ins Analoge** findet erst außerhalb des störenden PC-Gehäuses im abgeschirmten Interface statt!
