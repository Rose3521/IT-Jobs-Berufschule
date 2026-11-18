# ⚖️ LF 4 – Teil 3: Datenschutz (DSGVO) & Technische und Organisatorische Maßnahmen (TOM)

> **Lernfeld 4:** Schutzbedarfsanalyse im eigenen Arbeitsbereich durchführen  
> **AP1-Statistik:** *Datenschutz (DSGVO)* kam in **64 % aller AP1-Prüfungen (7 von 11 Prüfungen, 22 Punkte)** vor! Zusammen mit *TOM & Zutritts-/Zugangs-/Zugriffskontrolle (23 Punkte)* sind das **45 Punkte**!

---

## ✅ Checkliste der Lehrinhalte
- [ ] **Personenbezogene Daten (Art. 4 DSGVO)** und **besondere Kategorien (Art. 9 DSGVO)** erkennen
- [ ] Die **7 Grundsätze der DSGVO (Art. 5)** erklären
- [ ] **Rechte der betroffenen Personen** (Auskunft, Löschung, Berichtigung etc.) nennen
- [ ] Pflichten des Unternehmens (**Datenschutzbeauftragter ab 20 Personen**, **AVV**, **VVT**, **72-Stunden-Meldepflicht** bei Datenpannen) anwenden
- [ ] **TOMs** sicher in **Zutritts-, Zugangs-, Zugriffs-, Weitergabe-, Eingabe-, Auftrags- und Verfügbarkeitskontrolle** einteilen

---

## 📌 Lehrinhalte & Prüfungswissen

### 1. Grundlagen der DSGVO (Datenschutz-Grundverordnung) & BDSG

* **Wann gilt die DSGVO?**
  * Sobald **personenbezogene Daten** natürlicher (lebender) Personen verarbeitet werden (= alle Informationen, die sich auf eine identifizierte oder identifizierbare Person beziehen: *Name, Adresse, E-Mail, Telefonnummer, Geburtsdatum, Kontonummer, **IP-Adresse**, **Kfz-Kennzeichen**, **Personalnummer**, Standortdaten*). *(Reine Firmendaten einer GmbH wie `info@muster-gmbh.de` ohne Bezug zu einem Menschen sind keine personenbezogenen Daten!).*
  * **Besondere Kategorien personenbezogener Daten (Art. 9 DSGVO – besonders streng geschützt!):** Gesundheitsdaten, biometrische Daten (Fingerabdruck/Gesichtserkennung), genetische Daten, ethnische Herkunft, religiöse/politische Überzeugung, Gewerkschaftszugehörigkeit, Sexualleben.
* **Das Grundprinzip: „Verbot mit Erlaubnisvorbehalt“ (Art. 6 DSGVO):**
  * Jede Verarbeitung personenbezogener Daten ist **grundsätzlich verboten**, es sei denn, es liegt mindestens **eine gesetzliche Erlaubnis** vor:
    1. **Freiwillige, informierte Einwilligung** der Person (jederzeit widerrufbar, *Opt-in*!),
    2. **Erfüllung eines Vertrags** (z. B. Lieferadresse wird benötigt, um das bestellte Paket zu versenden),
    3. **Rechtliche Verpflichtung** (z. B. Übermittlung von Lohndaten an Finanzamt und Krankenkasse),
    4. **Lebenswichtige Interessen** (z. B. Notaufnahme im Krankenhaus),
    5. **Berechtigtes Interesse** des Unternehmens (sofern die Schutzinteressen des Betroffenen nicht überwiegen).
* **Die Grundsätze der Verarbeitung (Art. 5 DSGVO):**
  1. **Rechtmäßigkeit, Verarbeitung nach Treu und Glauben, Transparenz**
  2. **Zweckbindung:** Daten dürfen nur für den vorher klar festgelegten Zweck genutzt werden.
  3. **Datenminimierung (Datensparsamkeit):** Nur so viele Daten erheben, wie für den Zweck zwingend nötig sind *(für einen Newsletter braucht man nur die E-Mail-Adresse, nicht das Geburtsdatum oder die Handynummer!)*.
  4. **Richtigkeit:** Falsche Daten müssen unverzüglich berichtigt werden.
  5. **Speicherbegrenzung:** Daten müssen gelöscht werden, sobald der Zweck entfällt (und keine gesetzlichen Aufbewahrungsfristen nach HGB/AO mehr bestehen).
  6. **Integrität und Vertraulichkeit:** Schutz der Daten durch geeignete **TOMs**.
  7. **Rechenschaftspflicht:** Das Unternehmen muss die Einhaltung aller Punkte jederzeit nachweisen können!

---

### 2. Betroffenenrechte & Wichtige Unternehmenspflichten nach DSGVO
* **Rechte der Betroffenen (Kunden / Mitarbeiter):**
  * **Recht auf Auskunft (Art. 15):** Welche Daten sind über mich gespeichert, woher stammen sie und zu welchem Zweck?
  * **Recht auf Berichtigung (Art. 16)** unrichtiger Daten.
  * **Recht auf Löschung / „Recht auf Vergessenwerden“ (Art. 17)** *(Ausnahme: solange gesetzliche steuerliche Aufbewahrungsfristen von 6 bzw. 10 Jahren laufen, werden die Daten stattdessen für den normalen Zugriff **gesperrt / eingeschränkt**!)*.
  * **Recht auf Datenübertragbarkeit (Art. 20):** Herausgabe der eigenen Daten in einem gängigen, maschinenlesbaren Format (z. B. JSON, CSV, XML).
  * **Widerspruchsrecht (Art. 21)** & **Widerrufsrecht** einer erteilten Einwilligung.
* **Wichtige Pflichten für IT-Betriebe (Häufige Prüfungsfragen!):**
  * **Datenschutzbeauftragter (DSB – § 38 BDSG):** Muss zwingend benannt werden, wenn im Betrieb **in der Regel mindestens 20 Personen ständig mit der automatisierten Verarbeitung personenbezogener Daten beschäftigt sind** (oder unabhängig von der Mitarbeiterzahl, wenn besonders sensible Daten nach Art. 9 verarbeitet werden).
  * **Meldepflicht bei Datenpannen (Art. 33 DSGVO):** Bei einer Verletzung des Schutzes personenbezogener Daten (z. B. Hackerangriff, Ransomware, verlorener unverschlüsselter Laptop) muss das Unternehmen den Vorfall **unverzüglich, möglichst binnen 72 Stunden** nach Bekanntwerden an die zuständige **Datenschutz-Aufsichtsbehörde** (Landesdatenschutzbeauftragter) melden!
  * **Auftragsverarbeitungsvertrag (AVV nach Art. 28 DSGVO):** Wenn ein Unternehmen externe Dienstleister (z. B. Cloud-Hoster, externes Backup-Rechenzentrum, SaaS-Anbieter) mit der Verarbeitung von Kundendaten beauftragt, muss zwingend vorab ein **AV-Vertrag** geschlossen werden.

---

### 3. Technische und Organisatorische Maßnahmen (TOM) – Die 8 Kontrollbereiche

> **Prüfungs-Eselsbrücke für die Top 3:**  
> * **Zutritt** = Ich **trete** mit meinen Füßen durch eine **Tür in den Raum/das Gebäude**!  
> * **Zugang** = Ich **logge** mich am **System / PC / Netzwerk** ein!  
> * **Zugriff** = Ich bin schon eingeloggt und **greife** auf eine bestimmte **Datei / Datenbank / Ordner** zu!

| TOM-Kategorie | Was wird kontrolliert? | Konkrete Beispiele für die Prüfung |
| :--- | :--- | :--- |
| **1. Zutrittskontrolle** | **Physischer Raumzutritt** (Gebäude, Serverraum, Büro) | Sicherheitsschloss, elektronisches **Chipkarten-/Transpondersystem**, Biometrie-Scanner an der Serverraumtür, Alarmanlage, Videoüberwachung, Besucherbuch & Besucherausweise. |
| **2. Zugangskontrolle** | **Nutzung von IT-Systemen überhaupt** (Login an PC, Laptop, Smartphone, WLAN) | Starke **Passwortrichtlinie**, **Zwei-Faktor-Authentisierung (2FA/MFA)**, automatische Bildschirmsperre nach 5 Min. Inaktivität, Sperrung nach 5 Fehleingaben, BIOS-Passwort. |
| **3. Zugriffskontrolle** | **Berechtigungen auf Daten & Programme** (nach erfolgreichem Login!) | **Rollenbasiertes Berechtigungskonzept (RBAC)**, Prinzip der minimalen Rechte (**Least Privilege / Need-to-know**), NTFS-Ordnerberechtigungen (Buchhaltung sieht keine Entwicklungsdaten). |
| **4. Weitergabekontrolle** | **Schutz bei Transport & Übertragung** von Daten | **VPN-Tunnel**, **TLS/HTTPS**, E-Mail-Verschlüsselung (S/MIME, PGP), Festplatten-/USB-Stick-Verschlüsselung (**BitLocker**), Aktenvernichter (Schredder). |
| **5. Eingabekontrolle** | **Nachvollziehbarkeit:** Wer hat wann welche Daten eingegeben, geändert oder gelöscht? | Automatische **Protokollierung (Logging / Audit-Trails)** in Datenbanken, personalisierte Benutzerkonten (keine Sammel-Accounts wie `admin` oder `praktikant`!). |
| **6. Auftragskontrolle** | Weisungsgemäße Verarbeitung durch externe Dienstleister | Abschluss eines **Auftragsverarbeitungsvertrags (AVV)**, Prüfung der Sicherheitszertifikate (ISO 27001) des Cloud-Anbieters. |
| **7. Verfügbarkeitskontrolle** | Schutz vor **zufälliger Zerstörung oder Verlust** der Daten | **3-2-1-Backup-Konzept**, **RAID-Systeme**, **USV + Notstromaggregat**, Klimaanlage & Brandschutzanlage im Serverraum, Virenscanner. |
| **8. Trennungsgebot** | Getrennte Verarbeitung von Daten, die zu unterschiedlichen Zwecken erhoben wurden | **Mandantentrennung** bei verschiedenen Kunden, **strikte Trennung von Produktivsystem und Test-/Entwicklungssystem** (niemals mit echten Kundendaten testen!). |
