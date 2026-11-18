# 💻 BfK-I (LF 3) – LS 4: Erstkonfiguration von Netzwerkgeräten & Cisco IOS Netzwerklabor

> **Fach:** BfK-I (it.schule Stuttgart – 1. Lehrjahr) | **Lernfeld 3:** Clients in Netzwerke einbinden  
> **AP1-Statistik:** *Netzwerkkonfiguration* brachte in den AP1-Prüfungen (2025H, 2026F) **17 Punkte**!

---

## ✅ Checkliste der Lehrinhalte (LS 4 & Cisco Labor)
- [ ] **Konsolenverbindung (Out-of-Band):** Rollover-Kabel, Console-Port, Terminal-Parameter (`9600-8-N-1`)
- [ ] **Die 4 Speicher im Cisco-Gerät:** RAM (`running-config`), NVRAM (`startup-config`), Flash (IOS), ROM (POST/ROMMON)
- [ ] **Die 4 Cisco-IOS-Befehlsmodi:** User EXEC (`>`), Privileged EXEC (`#`), Global Config (`(config)#`), Sub-Config (`(config-if)#` / `(config-line)#`)
- [ ] **Erstkonfigurations-Befehle:** Hostname, Passwörter (`enable secret`), Verschlüsselung, Banner, SSH/VTY, IP-Vergabe, `no shutdown`, Speichern
- [ ] **Wichtige `show`-Diagnosebefehle** für Fehlersuche im Labor und in der Klassenarbeit

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Erstzugang über Konsole & Speicherarchitektur
* **Warum braucht man für die Erstkonfiguration ein Konsolenkabel?**
  * Ein fabrikneuer Managed Switch oder Router besitzt ab Werk **keine IP-Adresse** und keinen aktivierten SSH-Dienst.
  * Daher verbindet man den PC per **Konsolenkabel (Rollover-Kabel RJ45-auf-RS232 oder USB-Console)** direkt mit dem **`Console`-Port** des Geräts und öffnet ein Terminalprogramm (**PuTTY** / Packet Tracer Terminal) mit den Standardwerten **`9600 Baud, 8 Datenbits, Keine Parität, 1 Stoppbit, Keine Flusssteuerung (9600-8-N-1)`**.

| Speicherbaustein | Flüchtig? | Was liegt darin? | Wichtiger Hinweis für das Labor / die KA |
| :--- | :---: | :--- | :--- |
| **RAM** | **Ja (flüchtig!)** | **`running-config`** (aktuell laufende Konfiguration), Routing-Tabelle, ARP-/MAC-Cache. | Alle Befehle, die du im `(config)#`-Modus eintippst, landen **sofort im RAM**, gehen aber beim Ausschalten verloren, wenn du nicht speicherst! |
| **NVRAM** | **Nein** | **`startup-config`** (gespeicherte Startkonfiguration). | Wird mit **`copy running-config startup-config`** (oder `write memory`) aus dem RAM hierher gesichert. |
| **Flash** | **Nein** | Das **Cisco IOS Betriebssystem-Image** (`.bin`-Datei). | Entspricht der SSD/Festplatte eines PCs. |
| **ROM** | **Nein** | Bootstrap-Programm, POST-Selbsttest, **ROMMON** (Notfall-System zur Passwort-Wiederherstellung). | Entspricht dem BIOS/UEFI eines PCs. |

---

### 2. Die 4 Cisco-IOS-Befehlsmodi im Überblick

| Modus | Prompt | Befehl zum Eintreten | Befehl zum Verlassen | Zweck |
| :--- | :--- | :--- | :--- | :--- |
| **User EXEC Mode** | `Switch>` / `Router>` | *(Startmodus nach Login)* | `logout` / `exit` | Eingeschränkter Lesemodus (`ping`, Basis-`show`). |
| **Privileged EXEC Mode** | `Switch#` / `Router#` | **`enable`** *(kurz: `en`)* | `disable` / `exit` | Vollständiger Diagnosemodus (**alle `show`-Befehle**, `copy`, `reload`). |
| **Global Config Mode** | `Switch(config)#` | **`configure terminal`** *(kurz: `conf t`)* | `exit` / `end` | Globale Geräteeinstellungen ändern (Hostname, Banner, Passwörter). |
| **Interface / Line Mode** | `Router(config-if)#`<br>`Switch(config-line)#` | **`interface g0/0`** bzw.<br>**`line console 0`** | `exit` (1 Stufe zurück)<br>**`end`** / `Strg+Z` (direkt zu `#`) | Einen einzelnen Port oder einen Konsolen-/SSH-Zugang konfigurieren. |

---

### 3. Vollständige Befehlsreferenz für die Erstkonfiguration (LS 4)

```text
! 1. In den globalen Konfigurationsmodus wechseln:
Router> enable
Router# configure terminal

! 2. Hostname vergeben & DNS-Suche bei Tippfehlern abschalten:
Router(config)# hostname R1-Stuttgart
R1-Stuttgart(config)# no ip domain-lookup

! 3. Privilegierten Modus mit gehashtem Passwort schützen (NICHT "enable password" nutzen!):
R1-Stuttgart(config)# enable secret Sicher123!

! 4. Alle Klartext-Passwörter in der Konfigurationsdatei verschlüsseln:
R1-Stuttgart(config)# service password-encryption

! 5. Rechtlichen Warnhinweis (Message of the Day) vor dem Login setzen:
R1-Stuttgart(config)# banner motd #Zutritt nur fuer autorisierte Administratoren!#

! 6. Konsolenzugang (Line Console 0) mit Passwort schützen:
R1-Stuttgart(config)# line console 0
R1-Stuttgart(config-line)# password Konsole123
R1-Stuttgart(config-line)# login
R1-Stuttgart(config-line)# logging synchronous
R1-Stuttgart(config-line)# exit

! 7. Fernwartungszugang (Line VTY 0 4 bzw. 0 15) absichern:
R1-Stuttgart(config)# line vty 0 4
R1-Stuttgart(config-line)# password Fernzugriff123
R1-Stuttgart(config-line)# login
R1-Stuttgart(config-line)# transport input ssh
R1-Stuttgart(config-line)# exit

! 8. Router-Schnittstelle mit IPv4 + IPv6 konfigurieren und EINSCHALTEN:
R1-Stuttgart(config)# interface GigabitEthernet 0/0
R1-Stuttgart(config-if)# description Verbindung_zu_LAN_Abteilung_A
R1-Stuttgart(config-if)# ip address 192.168.10.1 255.255.255.0
R1-Stuttgart(config-if)# ipv6 address 2001:db8:10::1/64
R1-Stuttgart(config-if)# no shutdown
R1-Stuttgart(config-if)# end

! 9. Am Switch: Virtuelles Management-Interface (SVI) & Standardgateway setzen:
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.10.2 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# ip default-gateway 192.168.10.1
Switch(config)# end

! 10. WICHTIG: Konfiguration dauerhaft vom RAM in das NVRAM kopieren!
R1-Stuttgart# copy running-config startup-config
```

* **Einen versehentlichen Befehl wieder löschen:** Setze im selben Konfigurationsmodus einfach das Wort **`no`** vor den Befehl (z. B. `no ip address` oder `no banner motd`)!

---

### 4. Wichtige `show`-Befehle für die Diagnose
* `show ip interface brief` (`sh ip int br`): Zeigt auf einen Blick alle Ports, ihre IPv4-Adresse sowie Status (`up` / `down` / `administratively down` = `no shutdown` vergessen!) und Protocol (`up` / `down`).
* `show running-config` (`sh run`): Zeigt die aktuelle Konfiguration im RAM.
* `show startup-config` (`sh start`): Zeigt die gespeicherte Konfiguration im NVRAM.
* `show ip route`: Zeigt die Routing-Tabelle des Routers.
* `show mac-address-table`: Zeigt die gelernten MAC-Adressen und zugehörigen Ports am Switch.
