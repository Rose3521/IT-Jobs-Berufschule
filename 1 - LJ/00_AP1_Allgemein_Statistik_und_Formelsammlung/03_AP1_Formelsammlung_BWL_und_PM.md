# 💰 Kaufmännische & Projektmanagement-Formelsammlung für die AP1

---

## 1. Nutzwertanalyse (NWA) – Platz 1 aller AP1-Einzelthemen (45 Punkte!)

$$\text{Gewichtete Punkte (Teilnutzwert)} = \frac{\text{Gewichtung (in \%)}}{100} \times \text{Punktebewertung (Skala z. B. 1–10)}$$
$$\text{Gesamtnutzwert eines Angebots} = \sum \text{Gewichtete Punkte aller Kriterien}$$

| Bewertungskriterium | Gewichtung ($g_i$) | Anbieter A: Punkte ($p_i$) | Anbieter A: Gewichtete Punkte ($g_i \cdot p_i$) | Anbieter B: Punkte ($p_i$) | Anbieter B: Gewichtete Punkte ($g_i \cdot p_i$) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **1. Anschaffungspreis** | $40\,\%$ ($0{,}4$) | $6$ | $0{,}4 \times 6 = \mathbf{2{,}4}$ | $9$ | $0{,}4 \times 9 = \mathbf{3{,}6}$ |
| **2. Leistung / Ausstattung** | $35\,\%$ ($0{,}35$) | $10$ | $0{,}35 \times 10 = \mathbf{3{,}5}$ | $6$ | $0{,}35 \times 6 = \mathbf{2{,}1}$ |
| **3. Support / Reaktionszeit** | $15\,\%$ ($0{,}15$) | $8$ | $0{,}15 \times 8 = \mathbf{1{,}2}$ | $4$ | $0{,}15 \times 4 = \mathbf{0{,}6}$ |
| **4. Energieeffizienz (Green IT)** | $10\,\%$ ($0{,}10$) | $7$ | $0{,}10 \times 7 = \mathbf{0{,}7}$ | $8$ | $0{,}10 \times 8 = \mathbf{0{,}8}$ |
| **Summe** | **$100\,\%$ ($1{,}0$)** | | **Gesamtnutzwert: $\mathbf{7{,}8}$** | | **Gesamtnutzwert: $\mathbf{7{,}1}$** |

* **Prüfungs-Checkliste für die NWA:**
  1. **K.O.-Kriterien (Muss-Kriterien)** vorab prüfen (wer sie nicht erfüllt, fliegt vor der Tabelle raus!).
  2. Summe der Gewichtungen muss exakt **$100\,\%$** ergeben.
  3. **Begründeten Antwortsatz** formulieren.
  4. **Kritik an der NWA:** Subjektive Vergabe von Gewichtungen und Punkten.

---

## 2. Kaufmännische Handelskalkulation (Schema)

```text
  Listeneinkaufspreis (netto)                     100 %
- Liefererrabatt                                  - ... %
---------------------------------------------------------
= Zieleinkaufspreis                               100 %   (neue Basis für Skonto!)
- Liefererskonto                                  - ... %
---------------------------------------------------------
= Bareinkaufspreis
+ Bezugskosten (Fracht, Versand, Verpackung, Zoll)
---------------------------------------------------------
= BEZUGSPREIS / Einstandspreis                    100 %   (neue Basis für HKZ!)
+ Handlungskostenzuschlag (HKZ / Gemeinkosten)    + ... %
---------------------------------------------------------
= SELBSTKOSTENPREIS                               100 %   (neue Basis für Gewinn!)
+ Gewinnzuschlag                                  + ... %
---------------------------------------------------------
= Barverkaufspreis                                 ... %  (Im Hundert = 100 % - Skonto - Provision!)
+ Kundenskonto (+ ggf. Vertreterprovision)        + ... %
---------------------------------------------------------
= Zielverkaufspreis                                ... %  (Im Hundert = 100 % - Kundenrabatt!)
+ Kundenrabatt                                    + ... %
---------------------------------------------------------
= LISTENVERKAUFSPREIS (netto)                     100 %
+ Umsatzsteuer (i. d. R. 19 %)                    + 19 %
---------------------------------------------------------
= BRUTTOVERKAUFSPREIS                             119 %
```

---

## 3. Amortisation, TCO / Kostenvergleich, Stundensatz & Zinsen

* **Statische Amortisationsrechnung (AP1: 9 Punkte):**
  $$\text{Amortisationszeit (in Jahren)} = \frac{\text{Anschaffungskosten (Investition in €)}}{\text{Jährliche Einsparung (in €/Jahr)}}$$
* **Kostenvergleich / Kritische Menge (Break-Even z. B. bei 2 Druckern):**
  $$\text{Gesamtkosten } K(x) = K_{\text{fix}} + k_{\text{var}} \cdot x \qquad \longrightarrow \qquad x_{\text{krit}} = \frac{\text{Anschaffung}_A - \text{Anschaffung}_B}{\text{Seitenpreis}_B - \text{Seitenpreis}_A}$$
* **Stundensatzkalkulation (AP1: 10 Punkte):**
  $$\text{Selbstkosten-Stundensatz} = \frac{\text{Jahrespersonalkosten} + \text{Gemeinkosten}}{\text{Produktive Jahresarbeitsstunden}}$$
* **Kaufmännische Zinsformel (AP1: 7 Punkte):**
  $$Z = \frac{K \cdot p \cdot t}{100 \cdot 360} \qquad (\text{mit } 1\text{ Bankmonat} = 30\text{ Tage}, 1\text{ Bankjahr} = 360\text{ Tage})$$

---

## 4. Netzplantechnik (Platz 4 aller AP1-Einzelthemen – 37 Punkte!)

```text
+---------------------------------------+
|  Vorgangs-Nr.  |       Dauer (D)      |
+---------------------------------------+
|          Bezeichnung / Name           |
+---------------------------------------+
|   FAZ   |  GP  |     FP     |   FEZ   |
|   SAZ   |      |            |   SEZ   |
+---------------------------------------+
```
1. **Vorwärtsrechnung (links $\rightarrow$ rechts):** Start $\text{FAZ} = 0$ | $\mathbf{\text{FEZ} = \text{FAZ} + D}$ | $\mathbf{\text{FAZ}_{\text{Nachfolger}} = \max(\text{FEZ aller direkten Vorgänger})}$
2. **Rückwärtsrechnung (rechts $\rightarrow$ links):** Ende $\text{SEZ} = \text{FEZ}$ | $\mathbf{\text{SAZ} = \text{SEZ} - D}$ | $\mathbf{\text{SEZ}_{\text{Vorgänger}} = \min(\text{SAZ aller direkten Nachfolger})}$
3. **Pufferzeiten & Kritischer Pfad:**
   * **Gesamtpuffer:** $\mathbf{\text{GP} = \text{SAZ} - \text{FAZ}} = \text{SEZ} - \text{FEZ}$
   * **Freier Puffer:** $\mathbf{\text{FP} = \min(\text{FAZ aller direkten Nachfolger}) - \text{FEZ}}$
   * **Kritischer Pfad:** Kette aller Vorgänge mit **$\text{GP} = 0$** und **$\text{FP} = 0$** (keine Verzögerung erlaubt!).
