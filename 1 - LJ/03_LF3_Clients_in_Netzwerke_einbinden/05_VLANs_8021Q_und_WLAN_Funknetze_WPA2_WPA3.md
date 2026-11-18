# 🔀 BfK-I (LF 3) – LS 6: Netzwerke effizient und sicher aufteilen (VLANs & WLAN)

> **Fach:** BfK-I (it.schule Stuttgart – im Screenshot grün markiert für AP1!) | **Lernfeld 3:** Clients in Netzwerke einbinden  
> **AP1-Statistik:** *WLAN (9 P.) & VLANs / Netzwerkkonfiguration (17 P.)* sind feste Bestandteile der AP1!

---

## ✅ Checkliste der Lehrinhalte (LS 6)
- [ ] Funktionsweise und Vorteile von **VLANs (IEEE 802.1Q)** erklären
- [ ] **Port-based (Untagged / Access-Port)** vs. **Tagged VLAN (Trunk-Port)** unterscheiden
- [ ] **Inter-VLAN-Routing (Router-on-a-Stick / Layer-3-Switch)** erläutern
- [ ] **WLAN (IEEE 802.11):** Frequenzbänder (**2,4 GHz vs. 5 GHz / 6 GHz**), **CSMA/CA** und **WPA2 vs. WPA3 (PSK vs. Enterprise)** vergleichen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. VLAN (*Virtual Local Area Network* nach IEEE 802.1Q)

* **Was macht ein VLAN?**
  * Teilt einen physischen Layer-2-Switch logisch in **mehrere komplett voneinander getrennte virtuelle Switches (eigene Broadcastdomänen!)** auf – z. B. `VLAN 10 (Buchhaltung)`, `VLAN 20 (Entwicklung)`, `VLAN 30 (Gäste-WLAN)`.
* **3 Hauptvorteile von VLANs (Beliebte Prüfungsfrage!):**
  1. **Höhere IT-Sicherheit:** Geräte in verschiedenen VLANs (z. B. Gäste und Buchhaltung) können auf Layer 2 **nicht miteinander kommunizieren**, selbst wenn sie am selben physischen Switch stecken.
  2. **Kleinere Broadcastdomänen & höhere Performance:** Ein Broadcast in `VLAN 10` wird nur an Ports von `VLAN 10` weitergeleitet und belastet nicht das gesamte Firmennetz.
  3. **Hohe Flexibilität:** Zieht ein Mitarbeiter in ein anderes Büro oder ein anderes Stockwerk um, muss kein Kabel im Serverraum umgesteckt werden – der Admin weist den Switch-Port einfach per Software dem passenden VLAN zu!

#### Untagged (Access-Port) vs. Tagged (Trunk-Port nach IEEE 802.1Q)

```text
[PC Abt. A] --- (Untagged / Access Port VLAN 10) ---> +----------+                      +----------+ <--- (Access Port VLAN 10) --- [Server Abt. A]
                                                      | Switch 1 | ===(802.1Q TRUNK)==> | Switch 2 |
[PC Abt. B] --- (Untagged / Access Port VLAN 20) ---> +----------+  (Tagged VLAN 10+20) +----------+ <--- (Access Port VLAN 20) --- [Drucker Abt. B]
```

| Port-Art | Verbindet... | Funktionsweise |
| :--- | :--- | :--- |
| **Access-Port (*Untagged*)** | **Switch $\leftrightarrow$ Endgerät** (PC, Drucker, Telefon) | Gehört fest zu **genau einem einzigen VLAN**. Die Ethernet-Frames auf diesem Kabel haben **keinen** VLAN-Tag, weil ein normaler PC mit VLAN-Tags nichts anfangen kann. |
| **Trunk-Port (*Tagged* nach IEEE 802.1Q)** | **Switch $\leftrightarrow$ Switch** (oder Switch $\leftrightarrow$ Router / Access Point / VMware-Host) | Überträgt den Datenverkehr **mehrerer VLANs gleichzeitig über ein einziges Kabel**! Dazu schiebt der Switch beim Senden einen **4 Byte großen 802.1Q-Header (mit der 12-Bit VLAN-ID `1–4094`)** in den Ethernet-Frame und entfernt ihn wieder, bevor der Frame über einen Access-Port an den Ziel-PC geht. |

* **Inter-VLAN-Routing:** Da jedes VLAN ein eigenes IP-Subnetz und eine eigene Broadcastdomäne bildet, können zwei verschiedene VLANs **nur über einen Router (z. B. per *Router-on-a-Stick* mit Sub-Interfaces am Trunk-Port) oder einen Layer-3-Switch** miteinander kommunizieren (wo dann eine Firewall regeln kann, wer mit wem sprechen darf!).

---

### 2. WLAN (IEEE 802.11) – Frequenzen, Zugriffsverfahren & Sicherheit

* **Medienzugriffsverfahren im WLAN: CSMA/CA (*Carrier Sense Multiple Access / Collision Avoidance*):**
  * Im Kupferkabel (früher bei Hubs) nutzte man *CSMA/CD (Collision Detection)*, weil eine Netzwerkkarte beim Senden gleichzeitig horchen konnte, ob eine Kollision auftritt.
  * Im **WLAN (Funk / Halb-Duplex)** blendet das eigene starke Sendesignal der Antenne jedes schwache fremde Signal komplett aus – ein WLAN-Gerät kann also **während des Sendens NICHT hören, ob es kollidiert**!
  * Deshalb nutzt WLAN **CSMA/CA (Kollisionsvermeidung)**: Gerät horcht erst (*Listen before Talk*), wartet eine zufällige *Backoff-Zeit* ab, reserviert das Medium optional per **RTS/CTS** (*Request to Send / Clear to Send* gegen das *Hidden-Station-Problem*) und erwartet für jedes Datenpaket zwingend eine **Empfangsbestätigung (`ACK`)**.

| Kriterium | 2,4-GHz-Frequenzband | 5-GHz- / 6-GHz-Frequenzband (Wi-Fi 5 / 6 / 6E / 7) |
| :--- | :--- | :--- |
| **Reichweite & Wanddurchdringung** | **Sehr hoch** (langwelligeres Signal durchdringt Wände deutlich besser). | **Geringer** (kurzwelligeres Signal wird von Betonwänden stärker gedämpft). |
| **Max. Datenrate** | **Niedrig bis mittel** | **Sehr hoch** (breite $80/160/320\text{-MHz}$-Kanäle). |
| **Überlappungsfreie Kanäle & Störungen** | **Nur 3 überlappungsfreie Kanäle (Kanal 1, 6, 11)**; stark gestört durch Nachbar-WLANs, Bluetooth und Mikrowellen. | **Sehr viele überlappungsfreie Kanäle**; kaum Störquellen. |

* **WLAN-Sicherheit:**
  * **WPA2-PSK (Personal):** Alle Nutzer verwenden **denselben WLAN-Schlüssel** (Verschlüsselung mit **AES-CCMP**). *Nachteil im Betrieb:* Kündigt ein Mitarbeiter, müsste das Passwort auf hunderten Geräten geändert werden; anfällig für Offline-Wörterbuchangriffe auf den aufgezeichneten 4-Way-Handshake.
  * **WPA3-Personal (SAE – *Simultaneous Authentication of Equals*):** Moderner Nachfolger; verhindert Offline-Brute-Force-Angriffe auf das Passwort und bietet *Forward Secrecy*.
  * **WPA2 / WPA3-Enterprise (IEEE 802.1X mit RADIUS-Server):** **Pflicht im Unternehmensnetz!** Jeder Mitarbeiter meldet sich mit seinem **individuellen Active-Directory-Konto oder Client-Zertifikat** an. Verlässt ein Mitarbeiter die Firma, wird einfach sein Konto im RADIUS/AD gesperrt!
  * **Gäste-WLAN:** Muss zwingend über ein **eigenes VLAN** vom internen Firmennetz getrennt sein und **Client Isolation** aktiviert haben (damit Gäste untereinander nicht auf ihre Geräte zugreifen können).
