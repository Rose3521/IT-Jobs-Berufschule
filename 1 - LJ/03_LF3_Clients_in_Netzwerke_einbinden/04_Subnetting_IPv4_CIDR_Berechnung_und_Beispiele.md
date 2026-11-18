# 🧮 BfK-I (LF 3) – LS 5: Netzwerke in mehrere Subnetze aufteilen (Subnetting)

> **Fach:** BfK-I (it.schule Stuttgart – im Screenshot grün markiert für AP1!) | **Lernfeld 3:** Clients in Netzwerke einbinden  
> **AP1-Statistik:** *Subnetting* kam in **27 % aller AP1-Prüfungen** vor!

---

## ✅ Checkliste der Lehrinhalte (LS 5)
- [ ] Zweck von Subnetting erklären (Sicherheit, kleinere Broadcastdomänen, strukturierte Adressvergabe)
- [ ] **CIDR-Präfix (`/24` bis `/30`)** in dezimale **Subnetzmaske** umrechnen und umgekehrt
- [ ] **Anzahl der Subnetze ($2^s$)** und **Anzahl nutzbarer Hosts pro Subnetz ($2^h - 2$)** berechnen
- [ ] Für ein gegebenes Subnetz **Netz-ID**, **1. nutzbaren Host**, **letzten nutzbaren Host** und **Broadcast-Adresse** bestimmen
- [ ] **VLSM (*Variable Length Subnet Mask*)** für unterschiedlich große Abteilungen anwenden

---

## 📌 Lehrinhalte & Schritt-für-Schritt-Rechenweg

### 1. Die Grundformeln beim IPv4-Subnetting
Eine IPv4-Adresse hat insgesamt **$32\text{ Bit}$**. Davon gibt das **CIDR-Präfix `/n`** an, wie viele Bits von links für den **Netzanteil ($n$)** reserviert sind. Die restlichen Bits rechts sind die **Host-Bits ($h$)**:
$$h = 32 - n$$
1. **Gesamtanzahl der IP-Adressen pro Subnetz (Blockgröße / Sprungweite):**
   $$\text{Blockgröße} = 2^h$$
2. **Anzahl der nutzbaren Hosts (Endgeräte + Router-Port) pro Subnetz:**
   $$\text{Nutzbare Hosts} = 2^h - 2$$
   *(Warum **$- 2$**? Weil die **allererste Adresse** jedes Subnetzes die **Netz-ID** ist und die **allerletzte Adresse** jedes Subnetzes die **Broadcast-Adresse** ist – beide dürfen keinem PC zugewiesen werden!)*
3. **Dezimalwert der Subnetzmaske im aufgeteilten Oktett:**
   $$\text{Maskenwert} = 256 - \text{Blockgröße } (2^h)$$

---

### 2. Der ultimative Subnetting-Spickzettel (`/24` bis `/30`)

| CIDR | Subnetzmaske | Host-Bits ($h$) | Blockgröße ($2^h$) | **Nutzbare Hosts ($2^h - 2$)** | Anzahl Subnetze aus einem `/24`-Netz |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **`/24`** | `255.255.255.0` | $8$ | $256$ | **$254$** | $2^0 = 1$ |
| **`/25`** | `255.255.255.128` | $7$ | $128$ | **$126$** | $2^1 = 2$ |
| **`/26`** | `255.255.255.192` | $6$ | $64$ | **$62$** | $2^2 = 4$ |
| **`/27`** | `255.255.255.224` | $5$ | $32$ | **$30$** | $2^3 = 8$ |
| **`/28`** | `255.255.255.240` | $4$ | $16$ | **$14$** | $2^4 = 16$ |
| **`/29`** | `255.255.255.248` | $3$ | $8$ | **$6$** | $2^5 = 32$ |
| **`/30`** | `255.255.255.252` | $2$ | $4$ | **$2$** *(Ideal für Router-zu-Router-Strecken!)* | $2^6 = 64$ |

---

### 3. Zwei typische Prüfungsaufgaben mit Musterlösung

#### Aufgabentyp A: „Unterteilen Sie das Netz `192.168.20.0/24` für 3 Abteilungen mit je 45 PCs!“
1. **Wie viele Host-Bits ($h$) brauchen wir für mindestens 45 Hosts (+ 1 Router-Interface = 46 IPs)?**
   * $2^5 - 2 = 30$ (zu klein!)
   * $2^6 - 2 = \mathbf{62\text{ nutzbare Hosts}}$ $\rightarrow$ Passt! Wir brauchen also **$h = 6\text{ Host-Bits}$** (Blockgröße $2^6 = \mathbf{64}$).
2. **Neues Präfix & Subnetzmaske berechnen:**
   * $\text{Präfix } n = 32 - 6 = \mathbf{/26}$
   * $\text{Subnetzmaske im 4. Oktett} = 256 - 64 = \mathbf{192}$ $\rightarrow$ **`255.255.255.192`**
3. **Subnetztabelle aufstellen (Netz-ID springt immer um die Blockgröße $+64$):**

| Subnetz | Netz-ID (1. Adresse) | 1. nutzbarer Host (`Netz-ID + 1`) | Letzter nutzbarer Host (`Broadcast - 1`) | Broadcast-Adresse (`Nächste Netz-ID - 1`) |
| :--- | :--- | :--- | :--- | :--- |
| **1. Subnetz (Abt. 1)** | `192.168.20.0 /26` | `192.168.20.1` | `192.168.20.62` | `192.168.20.63` |
| **2. Subnetz (Abt. 2)** | `192.168.20.64 /26` | `192.168.20.65` | `192.168.20.126` | `192.168.20.127` |
| **3. Subnetz (Abt. 3)** | `192.168.20.128 /26` | `192.168.20.129` | `192.168.20.190` | `192.168.20.191` |
| **4. Subnetz (Reserve)** | `192.168.20.192 /26` | `192.168.20.193` | `192.168.20.254` | `192.168.20.255` |

#### Aufgabentyp B: „Ein PC hat die IP `10.0.5.140 /27`. Wie lauten Netz-ID und Broadcast-Adresse seines Subnetzes?“
1. `/27` bedeutet $h = 32 - 27 = 5\text{ Host-Bits}$ $\rightarrow$ **Blockgröße $= 2^5 = \mathbf{32}$** (Subnetzmaske `255.255.255.224`).
2. Vielfache von $32$ aufschreiben, bis man die `.140` einschließt:
   * $0 \rightarrow 32 \rightarrow 64 \rightarrow 96 \rightarrow \mathbf{128} \rightarrow \mathbf{160}$
   * Die Zahl $140$ liegt zwischen **$128$** und **$160$**!
3. **Ergebnis:**
   * **Netz-ID:** **`10.0.5.128 /27`**
   * **1. Host:** `10.0.5.129` | **Letzter Host:** `10.0.5.158`
   * **Broadcast-Adresse:** **`10.0.5.159`** (eins vor der nächsten Netz-ID `.160`!)
