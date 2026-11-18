# 🔍 LF 5 – Teil 5: SQL-Komplettwissen (SELECT, JOIN, GROUP BY, HAVING, DML & DDL)

> **Lernfeld 5:** Software zur Verwaltung von Daten anpassen (Datenbanken / SQL)  
> **AP1-Statistik:** *SQL* brachte in der AP1 **26 Punkte** (und in der AP2 FIAE über **450 Punkte**!).  
> *(Passend zu deinen Übungsaufgaben in `C:\P&D\Schule\KA SQL Prep`!)*

---

## ✅ Checkliste der Lehrinhalte
- [ ] **SQL-Sprachbereiche:** DDL (`CREATE`, `ALTER`, `DROP`), DML (`INSERT`, `UPDATE`, `DELETE`), DQL (`SELECT`), DCL (`GRANT`, `REVOKE`)
- [ ] **Reihenfolge der `SELECT`-Klauseln** fehlerfrei anwenden
- [ ] Filterbedingungen (`WHERE`, `AND`, `OR`, `BETWEEN`, `IN`, `LIKE '%...'`, `IS NULL`) formulieren
- [ ] **Aggregatfunktionen** (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`) mit **`GROUP BY`** und **`HAVING`** kombinieren
- [ ] **Tabellen verknüpfen (`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`)** über Primär- und Fremdschlüssel

---

## 📌 Lehrinhalte & SQL-Spickzettel

### 1. Die zwingende Reihenfolge einer `SELECT`-Abfrage

```sql
SELECT [DISTINCT] spalte1, AGGREGATFUNKTION(spalte2) AS aliasname
FROM tabelle1 t1
INNER JOIN tabelle2 t2 ON t1.fremdschluessel = t2.primaerschluessel
WHERE bedingung_fuer_einzelzeilen
GROUP BY spalte1
HAVING bedingung_fuer_aggregatfunktionen
ORDER BY aliasname DESC
LIMIT 10;
```

---

### 2. `WHERE`-Operatoren & Wildcards

| Operator | Beispiel | Bedeutung |
| :--- | :--- | :--- |
| **`=` / `<>` (bzw. `!=`)** | `WHERE Ort = 'Stuttgart'` | Exakte Gleichheit / Ungleichheit (Texte immer in einfachen Hochkommas `'...'`!). |
| **`AND` / `OR`** | `WHERE (Ort = 'Ulm' OR Ort = 'Stuttgart') AND Umsatz > 500` | **`AND` bindet stärker als `OR`!** Bei `OR`-Verknüpfungen immer Klammern setzen! |
| **`BETWEEN ... AND ...`** | `WHERE Preis BETWEEN 10.00 AND 50.00` | Inklusive der beiden Grenzwerte (`Preis >= 10.00 AND Preis <= 50.00`). |
| **`IN (...)`** | `WHERE Abteilung IN ('IT', 'Vertrieb', 'HR')` | Prüft, ob der Wert in der Liste vorkommt. |
| **`LIKE` mit `%` und `_`** | `WHERE Nachname LIKE 'M%er'`<br>`WHERE ArtikelNr LIKE 'A_9%'` | **`%`** = beliebig viele Zeichen (auch 0)<br>**`_`** = genau **1 einzelnes** beliebiges Zeichen! |
| **`IS NULL` / `IS NOT NULL`** | `WHERE Austrittsdatum IS NULL` | Prüft auf leere Datenbankfelder (**NIEMALS** `= NULL` schreiben!). |

---

### 3. Aggregatfunktionen, `GROUP BY` & `HAVING` (Prüfungsklassiker!)

* **Die 5 Aggregatfunktionen:**
  * **`COUNT(*)`:** Zählt alle Zeilen einer Gruppe (inkl. `NULL`-Werten).
  * **`COUNT(spalte)`:** Zählt nur die Zeilen, in denen `spalte` **nicht `NULL`** ist!
  * **`SUM(spalte)`:** Berechnet die Summe aller Werte.
  * **`AVG(spalte)`:** Berechnet den Durchschnittswert (*Average*).
  * **`MIN(spalte)` / `MAX(spalte)`:** Kleinster bzw. größter Wert.
* **Goldene Regel zu `GROUP BY`:**
  * Sobald im `SELECT` neben einer Aggregatfunktion (wie `SUM` oder `COUNT`) eine normale Spalte steht (z. B. `Abteilungsname`), **MUSS diese normale Spalte zwingend im `GROUP BY` aufgeführt werden**!
* **Unterschied zwischen `WHERE` und `HAVING`:**
  * **`WHERE`** filtert **einzelne Datensätze VOR dem Gruppieren** (darf **keine** Aggregatfunktionen enthalten!).
  * **`HAVING`** filtert **fertige Gruppen NACH dem `GROUP BY`** anhand des Ergebnisses einer Aggregatfunktion (z. B. *„Zeige nur Abteilungen an, die mehr als 5 Mitarbeiter haben“* $\rightarrow$ `GROUP BY Abteilung HAVING COUNT(*) > 5`).

---

### 4. Tabellen verknüpfen mit `JOIN` (`INNER JOIN` vs. `LEFT JOIN`)

Angenommen, wir haben die Tabellen `Kunden (Kunden_ID, Name, Ort)` und `Bestellungen (Bestell_ID, Datum, Betrag, FK_Kunden_ID)`:

```sql
-- 1. INNER JOIN: Zeigt NUR Kunden an, die mindestens eine Bestellung aufgegeben haben:
SELECT k.Name, b.Bestell_ID, b.Datum, b.Betrag
FROM Kunden k
INNER JOIN Bestellungen b ON k.Kunden_ID = b.FK_Kunden_ID;

-- 2. LEFT JOIN: Zeigt ALLE Kunden an (auch Neukunden OHNE Bestellung -> rechts steht dann NULL):
SELECT k.Name, COUNT(b.Bestell_ID) AS Anzahl_Bestellungen
FROM Kunden k
LEFT JOIN Bestellungen b ON k.Kunden_ID = b.FK_Kunden_ID
GROUP BY k.Kunden_ID, k.Name;

-- 3. Beliebter Prüfungstrick: "Welche Kunden haben noch NIE etwas bestellt?"
SELECT k.Kunden_ID, k.Name
FROM Kunden k
LEFT JOIN Bestellungen b ON k.Kunden_ID = b.FK_Kunden_ID
WHERE b.Bestell_ID IS NULL;
```

---

### 5. Daten ändern & Tabellen anlegen (DML & DDL – AP1 & AP2 Pflichtwissen!)

```sql
-- 1. Neuen Datensatz einfügen (INSERT INTO):
INSERT INTO Kunden (Kunden_ID, Name, Ort)
VALUES (105, 'Lisa Schmidt', 'Karlsruhe');

-- 2. Bestehende Datensätze ändern (UPDATE - IMMER ans WHERE denken, sonst werden ALLE Zeilen überschrieben!):
UPDATE Artikel
SET Einzelpreis = Einzelpreis * 1.05
WHERE Kategorie = 'Monitore';

-- 3. Datensätze löschen (DELETE - IMMER ans WHERE denken!):
DELETE FROM Bestellungen
WHERE Bestelldatum < '2020-01-01';

-- 4. Neue Tabelle mit Primärschlüssel und Fremdschlüssel anlegen (CREATE TABLE):
CREATE TABLE Bestellungen (
    Bestell_ID INT PRIMARY KEY AUTO_INCREMENT,
    Bestelldatum DATE NOT NULL,
    Betrag DECIMAL(10, 2) NOT NULL,
    FK_Kunden_ID INT NOT NULL,
    FOREIGN KEY (FK_Kunden_ID) REFERENCES Kunden(Kunden_ID)
);
```
