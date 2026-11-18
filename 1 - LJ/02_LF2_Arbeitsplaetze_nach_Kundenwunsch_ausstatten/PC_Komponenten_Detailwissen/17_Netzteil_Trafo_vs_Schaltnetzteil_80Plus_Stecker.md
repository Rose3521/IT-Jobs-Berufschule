# ⚡ LS 1 – Thema 17: Netzteil (Trafo- vs. Schaltnetzteil, Spannungen, Wirkungsgrad, 80 PLUS & Stecker)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten  
> **AP1-Statistik:** *Leistungsaufnahme & Stromkosten berechnen* brachten in der AP1 bisher **28 Punkte**!

---

## ✅ Lehrinhalte nach LS 1 & AP1
- [ ] **Leistungsangabe ($P_{\text{ab}}$ vs. $P_{\text{zu}}$)**, Strom, Ausgangsspannungen (`+12 V`, `+5 V`, `+3,3 V`)
- [ ] **Wirkungsgrad ($\eta$)** & **80-PLUS-Effizienz-Zertifikate**
- [ ] Funktionsprinzip im Vergleich: **Klassisches Trafo-Netzteil vs. modernes Schaltnetzteil (SMPS)**
- [ ] **Lebensdauer** (Elektrolytkondensatoren / Temperatur), Formfaktoren (ATX, SFX), Stecker & Schutzschaltungen

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Aufgaben & Ausgangsspannungen eines PC-Netzteils
Das PC-Netzteil (**PSU – *Power Supply Unit***) wandelt die **$230\text{ V}$ Wechselspannung ($50\text{ Hz}$ AC)** aus dem Stromnetz in galvanisch getrennte, präzise geregelte **Kleinspannungen (Gleichspannung DC)** um:
* **`+12 V`-Schiene (Wichtigste Hauptschiene – über 90 % der Gesamtleistung!):** Versorgt die leistungshungrigsten Komponenten: **CPU**, **Grafikkarte**, Gehäuselüfter und Festplattenmotoren. *(Qualitätsmerkmal: Ein gutes $600\text{-W}$-Netzteil muss auf der $+12\text{V}$-Schiene allein fast die vollen $600\text{ W}$ liefern können, z. B. $12\text{ V} \times 50\text{ A} = 600\text{ W}$!).*
* **`+5 V`-Schiene:** Versorgt **USB-Ports**, SATA-Laufwerke und Chipsatz-Logik.
* **`+3,3 V`-Schiene:** Versorgt **M.2-NVMe-SSDs** und Mainboard-Kleinspannungen.
* **`+5 VSB` (*Standby-Spannung*):** Liefert auch bei heruntergefahrenem PC dauerhaft $5\text{ V}$ (ca. $2\text{–}3\text{ A}$), damit der Einschaltknopf am Gehäuse, *Wake-on-LAN* und das Laden per USB im Standby funktionieren.
* **`Power_Good`-Signal (`PWR_OK`):** Eine Steuerleitung, über die das Netzteil dem Mainboard beim Einschalten erst dann das Startsignal gibt, wenn alle Spannungen nach wenigen Millisekunden stabil aufgebaut sind!

---

### 2. Funktionsprinzip: Trafo-Netzteil vs. Schaltnetzteil (SMPS)

Warum wiegt ein modernes $750\text{-Watt}$-PC-Netzteil nur ca. $1{,}5\text{ kg}$, während ein klassischer $750\text{-Watt}$-Eisenkerntrafo über $12\text{ kg}$ wiegen würde?

```text
1. KLASSISCHES LINEARES TRAFO-NETZTEIL (50 Hz):
[230V / 50Hz AC] ---> [Riesiger, schwerer 50-Hz-Eisenkerntrafo] ---> [Gleichrichter + Glättung] ---> [Linearregler (verheizt Überschuss!)] ---> [12V DC]

2. MODERNES SCHALTNETZTEIL (SMPS - Switched-Mode Power Supply):
[230V / 50Hz AC] ---> [Primär-Gleichrichter + PFC (~380V DC)] ---> [Schnelle Schalttransistoren zerhacken in ~100.000 Hz (100 kHz)!]
                                                                                  ↓
[12V DC Ausgang] <--- [Sekundär-Gleichrichter + Glättung] <--- [Winziger Hochfrequenz-Ferrit-Trafo (galvanische Trennung)]
        |                                                                         ↑
        +-------> [Optokoppler-Rückkopplung regelt Pulsweite (PWM) bei Lastwechsel sofort nach!]
```

| Merkmal | Klassisches Trafo-Netzteil (Linearnetzteil) | Modernes Schaltnetzteil (SMPS) |
| :--- | :--- | :--- |
| **Arbeitsfrequenz am Trafo** | Niedrige **$50\text{ Hz}$ Netzfrequenz** | Hohe **$50.000\text{ bis }200.000\text{ Hz}$ ($50\text{–}200\text{ kHz}$)** |
| **Größe & Gewicht des Trafos** | **Riesig und extrem schwer** (je niedriger die Frequenz, desto größer muss der Eisenkern sein!). | **Winzig und sehr leicht** (da bei $100\text{ kHz}$ pro Schwingung nur ein Tausendstel der Energie übertragen werden muss, reicht ein kleiner Ferritkern!). |
| **Spannungsregelung & Wirkungsgrad** | Überschüssige Spannung wird an einem Widerstand/Transistor **direkt in Wärme verheizt** ($\eta \approx \mathbf{40\text{–}60\,\%}$). | Regelung erfolgt fast verlustfrei über die Einschaltdauer der Impulse (**PWM – *Pulsweitenmodulation***) $\rightarrow$ **sehr hoher Wirkungsgrad ($\eta \approx \mathbf{85\text{–}94\,\%}$)**! |

---

### 3. Wirkungsgrad ($\eta$), 80-PLUS-Zertifikate & Berechnung

$$\text{Wirkungsgrad } \eta = \frac{P_{\text{ab}} \text{ (Abgegebene Sekundärleistung an die PC-Teile)}}{P_{\text{zu}} \text{ (Aufgenommene Primärleistung aus der Steckdose)}} \quad \Longleftrightarrow \quad P_{\text{zu}} = \frac{P_{\text{ab}}}{\eta}$$
$$\text{Verlustleistung (Abwärme im Netzteil)} = P_{\text{zu}} - P_{\text{ab}}$$

* **80-PLUS-Zertifizierung (bei $230\text{ V}$ EU-Netz und $50\,\%$ Auslastung, wo ein Netzteil am effizientesten arbeitet):**
  * **80 PLUS Bronze:** $\ge 88\,\%$ | **Silver:** $\ge 90\,\%$ | **Gold:** **$\ge 92\,\%$** | **Platinum:** $\ge 94\,\%$ | **Titanium:** **$\ge 96\,\%$** (hat als einziges Zertifikat auch eine Pflichtvorgabe für **10 % Niedriglast / Office-Leerlauf** von $\ge 90\,\%$!).

### 4. Lebensdauer, Formate, Stecker & Schutzschaltungen
* **Lebensdauer & Temperatur:** Das empfindlichste Verschleißteil im Schaltnetzteil sind die **Elektrolytkondensatoren (Elkos)** zur Spannungsglättung. Nach der **Arrhenius-Lebensdauerregel** halbiert sich die Lebensdauer eines Elkos bei **jeder Temperaturerhöhung um $10\text{ °C}$**! Hochwertige Netzteile nutzen daher japanische **$105\text{ °C}$-Kondensatoren** statt billiger $85\text{ °C}$-Typen.
* **Wichtigste Stecker:** **24-Pin ATX** (Mainboard), **4+4-Pin / 8-Pin EPS** (CPU-Strom), **6+2-Pin PCIe** bzw. **16-Pin 12V-2x6 / 12VHPWR** (Grafikkarten bis $600\text{ W}$), **15-Pin SATA** (Laufwerke).
* **Pflicht-Schutzschaltungen (Norm ATX 3.0/3.1):** **OCP** (Überstrom), **OVP / UVP** (Über-/Unterspannung), **OPP** (Überlast), **SCP** (Kurzschluss), **OTP** (Übertemperatur).

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Auf einem PC-Netzteil steht ‚600 Watt maximale Ausgangsleistung‘. Zieht dieses Netzteil im normalen Bürobetrieb (Word/Browser geöffnet) permanent 600 Watt aus der Steckdose? Und kann es unter Volllast sogar MEHR als 600 Watt aus der Steckdose ziehen?“*
   * **Lösung:**
     1. **Im Bürobetrieb:** Nein! Ein Schaltnetzteil liefert immer nur exakt so viel Leistung, wie die PC-Komponenten in dieser Sekunde tatsächlich anfordern (im Office-Leerlauf meist nur $30\text{–}50\text{ Watt}$).
     2. **Unter Volllast:** Ja! Die Nennleistung von „$600\text{ W}$“ gibt an, wie viel Leistung das Netzteil maximal **an die PC-Teile abgeben kann ($P_{\text{ab}} = 600\text{ W}$)**. Bei einem Wirkungsgrad von z. B. $\eta = 85\,\%$ muss es dafür aus der Steckdose $P_{\text{zu}} = \frac{600\text{ W}}{0{,}85} \approx \mathbf{706\text{ Watt}}$ aufnehmen (wovon $106\text{ W}$ als Wärme im Netzteil verloren gehen)!
2. **Frage 2:** *„Warum ist es lebensgefährlich, ein PC-Netzteil aufzuschrauben, selbst wenn man das Stromkabel bereits vor 5 Minuten aus der Steckdose gezogen hat?“*
   * **Lösung:** Im Primärkreis eines Schaltnetzteils sitzt ein großer **Primär-Elektrolytkondensator**, der im Betrieb auf rund **$325\text{ bis }400\text{ Volt}$ Gleichspannung** aufgeladen wird. Auch nach dem Ziehen des Netzsteckers kann dieser Kondensator seine tödliche Hochspannung noch über längere Zeit speichern!
