# ⚙️ LS 1 – Thema 15: Treiber (Treibermodell, Kernel- vs. User-Mode, Fehlerbilder & Updates)

> **Fach:** BfK-I (it.schule Stuttgart, Herr Gorel-Brwari) | **Lernfeld 2:** Arbeitsplätze nach Kundenwunsch ausstatten

---

## ✅ Lehrinhalte nach LS 1
- [ ] **Treibermodell:** Aufgabe eines Gerätetreibers, HAL (*Hardware Abstraction Layer*), **Kernel-Mode vs. User-Mode**
- [ ] Auswirkungen **fehlender oder nicht passender Treiber** (Symptome & Diagnose)
- [ ] **Treiber-Updates**, digitale Treibersignatur (WHQL) & Rollback

---

## 📌 Technische Funktionsweise & Expertenwissen

### 1. Das Treibermodell: Wie kommunizieren App, Betriebssystem und Hardware?

```text
+-------------------------------------------------------------------+
| ANWENDUNG (User-Mode, Ring 3)  z. B. Word, Spiel, Browser         |
+-------------------------------------------------------------------+
                                 ↕  (Standardisierte API-Aufrufe, z. B. DirectX / Win32 API)
+-------------------------------------------------------------------+
| BETRIEBSSYSTEM-KERNEL & HAL (Hardware Abstraction Layer)          |
+-------------------------------------------------------------------+
                                 ↕  (Gerätespezifische Übersetzung durch den Treiber!)
+-------------------------------------------------------------------+
| GERÄTETREIBER (Device Driver: .sys / .dll / Kernel-Modul)         |
+-------------------------------------------------------------------+
                                 ↕  (Registerzugriffe, Interrupts, DMA über PCIe/USB)
+-------------------------------------------------------------------+
| PHYSISCHE HARDWARE             z. B. Nvidia GPU, HP Laserdrucker  |
+-------------------------------------------------------------------+
```

* **Warum gibt es Treiber?**
  * Es gibt weltweit hunderttausende verschiedene Drucker-, Grafikkarten-, Mainboard- und Netzwerkkarten-Modelle, die alle intern unterschiedliche Steuerbefehle und Speicherregister nutzen.
  * Damit ein Softwareentwickler (z. B. von Microsoft Word) nicht für 10.000 verschiedene Drucker eigenen Code programmieren muss, spricht Word nur die **einheitliche Schnittstelle (API) des Betriebssystems** an. Der **Gerätetreiber des Hardware-Herstellers** arbeitet als **Dolmetscher** und übersetzt den allgemeinen Befehl des Betriebssystems in die exakten elektrischen Steuerkommandos der konkreten Hardware!
* **Kernel-Mode-Treiber (Ring 0) vs. User-Mode-Treiber (Ring 3):**
  * **Kernel-Mode-Treiber:** Laufen aus Geschwindigkeitsgründen mit **uneingeschränkten Systemrechten direkt im Betriebssystemkern** (z. B. Grafikkarten-, Chipsatz-, NVMe- und Netzwerktreiber). **Kritische Gefahr:** Enthält ein Kernel-Mode-Treiber einen Programmierfehler oder greift auf falschen Speicher zu, stürzt das gesamte Betriebssystem sofort mit einem **Bluescreen (BSOD – *Blue Screen of Death*)** ab!
  * **User-Mode-Treiber (UMDF):** Laufen in einem geschützten, isolierten Speicherbereich ohne direkte Kernel-Rechte (z. B. Druckertreiber, USB-Kameras, Smartcard-Leser). Stürzt ein User-Mode-Treiber ab, startet Windows einfach den Treiberdienst im Hintergrund neu, **ohne dass der PC abstürzt**!

---

### 2. Fehlende vs. Nicht passende Treiber (Fehlerbilder in der Praxis)

| Szenario | Was passiert technisch / Welche Symptome sieht der Kunde? |
| :--- | :--- |
| **1. Treiber fehlt komplett (und OS hat keinen Standardtreiber)** | Das Gerät ist komplett **funktionslos** (z. B. WLAN-Karte oder Spezialscanner wird nicht erkannt). Im Windows-**Geräte-Manager** erscheint das Gerät unter *„Andere Geräte“* mit einem **gelben Warndreieck (`!`)** und dem Status *„Die Treiber für dieses Gerät wurden nicht installiert (Code 28)“*. *(Lösung: Anhand der **Hardware-ID** `VEN_xxxx&DEV_xxxx` den passenden Herstellertreiber installieren).* |
| **2. Spezifischer Treiber fehlt, aber OS nutzt einen „Generischen Standardtreiber“** | Das Gerät funktioniert nur mit **minimalen Basisfunktionen**! *Klassisches Beispiel Grafikkarte:* Ohne Nvidia-/AMD-Treiber lädt Windows den *„Microsoft Basic Display Adapter“* $\rightarrow$ Bild ist zwar da, aber **geringe Auflösung, nur 60 Hz, ruckelnde Fenster, keine 3D-Beschleunigung, kein zweiter Monitor und kein Standby-Modus möglich**! *Beispiel Drucker:* Druckt nur Schwarz-Weiß und kann das Dupleomodul (beidseitiges Drucken) nicht ansprechen. |
| **3. Falscher / Nicht passender Treiber installiert** | * **Falsche Bit-Architektur:** Ein **32-Bit-Treiber kann auf einem 64-Bit-Betriebssystem grundsätzlich NICHT im Kernel geladen werden** (und umgekehrt)!<br>* **Treiber für falsches Hardware-Modell / veraltetes OS:** Gerät startet nicht (*Code 10*), produziert Grafikfehler/Tonknackser oder führt sofort beim Systemstart zu einem **Bluescreen (BSOD, z. B. `IRQL_NOT_LESS_OR_EQUAL`)**. |

---

### 3. Treiber-Updates, WHQL-Signatur & Rollback
* **Warum sollte man Treiber aktualisieren?**
  1. **Sicherheit:** Schließen kritischer Sicherheitslücken im Kernel-Mode-Treiber (Schutz vor *Privilege Escalation* / *Bring Your Own Vulnerable Driver*-Angriffen).
  2. **Stabilität & Bugfixes:** Behebung von Abstürzen nach großen Windows-Updates.
  3. **Performance:** Optimierungen für neue Anwendungen/Spiele.
* **Digitale Treibersignatur (WHQL – *Windows Hardware Quality Labs*):**
  * Ein 64-Bit-Windows verweigert standardmäßig die Installation von Kernel-Mode-Treibern, die keine gültige **digitale Signatur** besitzen. Damit wird verhindert, dass Malware (Rootkits) sich als Gerätetreiber tarnt und im Kernel-Modus einnistet.
* **Was tun, wenn der PC NACH einem Treiber-Update plötzlich spinnt?**
  * Im Geräte-Manager unter *Eigenschaften $\rightarrow$ Treiber* auf **„Vorheriger Treiber“ (*Roll Back Driver*)** klicken (stellt mit einem Klick die vorherige funktionierende Version wieder her) – oder falls Windows gar nicht mehr normal startet, in den **Abgesicherten Modus (*Safe Mode*)** booten (lädt nur minimale Standardtreiber) und den fehlerhaften Treiber dort deinstallieren!

---

## 🧠 KA- & Präsentations-Denkfragen (inkl. Musterlösung)

1. **Frage 1:** *„Auf einem 64-Bit-Windows-11-PC laufen problemlos alte 32-Bit-Anwendungsprogramme (wie ein altes Kalkulationsprogramm über die WOW64-Emulationsschicht). Warum weigert sich dasselbe 64-Bit-Windows aber strikt, den 32-Bit-Treiber für einen älteren Etikettendrucker zu installieren?“*
   * **Lösung:** Normale Anwendungsprogramme laufen im isolierten **User-Mode**, wo Windows über das Subsystem **WOW64 (*Windows 32-bit on Windows 64-bit*)** 32-Bit-Aufrufe übersetzen kann. Ein **Kernel-Mode-Treiber** wird jedoch direkt in den **64-Bit-Betriebssystemkern (Kernel)** geladen – und in einem 64-Bit-Kernel kann technisch kein 32-Bit-Code ausgeführt werden. Für ein 64-Bit-Betriebssystem sind daher zwingend 64-Bit-Treiber erforderlich!
2. **Frage 2:** *„Ein Mitarbeiter lädt sich für seine neue Grafikkarte einen Treiber von einer dubiosen Drittanbieter-Webseite herunter. Windows blockiert die Installation mit einer roten Warnmeldung über eine fehlende digitale Signatur. Warum ist es extrem gefährlich, diese Prüfung abzuschalten und den unsignierten Treiber trotzdem zu installieren?“*
   * **Lösung:** Die **digitale Signatur** garantiert über ein kryptografisches Zertifikat (**Authentizität & Integrität**), dass der Treiber wirklich original vom Hersteller (und von Microsoft WHQL geprüft) stammt und nachträglich nicht verändert wurde. Da ein Grafiktreiber im **Kernel-Mode (Ring 0)** mit den höchsten Rechten des gesamten Systems läuft, hätte ein manipulierter Treiber (**Rootkit**) sofort volle Kontrolle über den gesamten PC, könnte jeden Virenscanner aushebeln und sämtliche Passwörter mitlesen!
