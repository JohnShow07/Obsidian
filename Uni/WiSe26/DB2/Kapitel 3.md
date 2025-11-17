#DB2 #Kapitel3

# Datenbankdefinitionssprachen

*SQL-DDL:* Teil der Standartsprache SQL für relationale Datenbanksysteme
- Umfasst alle Klauseln von SQL, die mit Def. von
	- Typen
	- Wertebereichen
	- Relationenschemata
	- Integritätsbedingungen
	zu tun haben.

## SQL als Def. Sprache

> externe Ebene (View-Ebene: Welche Daten ein Nutzer sieht)
- create view
- drop view
> konzeptionelle Ebene (Logische Ebene: Wie ist die Datenbank logisch aufgebaut?)
- create table (domain in SQL-92)
- alter table (domain in SQL-92)
- drop table (domain in SQL-92)
> Interne Ebene (Physische Ebene: physische Organisation der Daten)
- create index
- alter index
- drop index

### create table

```sql
create table basisrelationenname (
	spaltenname 1 wertebereich 1 [not null],
	...
	spaltenname k wertebereich k [not null])
```

> **Datentypen**

| Datentyp               | Beschreibung                           |
| ---------------------- | -------------------------------------- |
| integer / int          | Ganze Zahl                             |
| smallint               | Kleine ganze Zahl                      |
| float(p)               | Gleitkommazahl mit Präzision p         |
| decimal(p, q)          | Zahl mit p Stellen, q Nachkommastellen |
| numeric(p, q)          | Wie decimal, exakt                     |
| character(n) / char(n) | String fester Länge n                  |
| varchar(n)             | String variabler Länge bis n           |
| bit(n)                 | Bitfolge fester Länge n                |
| bit varying(n)         | Bitfolge variabler Länge bis n         |
| date                   | Datum                                  |
| time                   | Uhrzeit                                |
| timestamp              | Datum und Uhrzeit                      |
BSP Code:

```sql
create table Bücher (
	ISBN char(10) not null, -- Nullwerte hier ausgeschlossen
	Titel varchar(200),
	Verlagsname varchar(30)
)
```

#### IEF (Integrity Enhancement Feature) -> SQL-89 Level 2
- Erweiterung der Integritätsregeln
- Zusatz zu SQL-89 -> SQL-89 Level 2:
	- darf: Primär- und Fremdschlüssel definieren

```sql
create table B¨ucher (
	ISBN char(10) not null,
	Titel varchar(200),
	Verlagsname varchar(30),
	primary key (ISBN), -- not null implizit durch primary key-Klausel
	foreign key (Verlagsname)
	references Verlage (Verlagsname)
)
```

#### Erweiterungen in SQL-92
- `default`- Klausel: *Defaultwerte für Attribute
- ``create domain``-Anweisung: *benutzerdefinierte Wertebereiche*
- `check`-Klausel: weitere *lokale Integritätsbedingungen* innerhalb der zu definierenden Wertebereiche, Attribute und Relationenschemata
> **Erstellung eines Wertebereichs**

```sql
create domain Gebiete varchar(20) -- neuer Datentyp "Gebiete"
	default ’Informatik’ -- Default Wert "Informatik"
```

```sql
create table Vorlesungen (
	V Bezeichnung varchar(80) not null,
	SWS smallint,
	Semester smallint,
	Studiengang Gebiete ) -- nutzt den selbst definierten Domäne "Gebiete"
	-- Wenn kein Wert eingetragen wird -> automatisch "Informatik"
```

```sql
create table Mitarbeiter (
	PANr integer not null,
	AngNr char(10) not null,
	Fachbereich Gebiete, -- genauso hier, Typ "Gebiete". wenn nichts eingetragen -> "Informatik"
	Gehalt decimal(10,2),
	Raum integer,
	Einstellung date )
```

**Vorteile**:
✔ *Konsistente* Datentypen zu garantieren
	z. B. überall, wo ein Studiengang/Fachbereich vorkommt, gelten dieselben Regeln.
✔ *Standardwerte* festzulegen
	z. B. Default „Informatik“, wenn kein anderer Fachbereich angegeben wird.
✔ Datenbanklogik *wiederverwendbar* zu machen
	Man definiert es einmal und benutzt es überall.

>Integritätsbedingungen mit`check`

```sql
create domain Gebiete varchar(20)
	default ’Informatik’ 
	check (
		value in ( ’Informatik’, ’Mathematik’, ’Physik’, ’Chemie’, ’Biologie’) )
	-- Domain "Gebiete" darf nur diese 5 Werte annehmen, alles andere verboten.
```

```sql
create table Vorlesungen (
    V_Bezeichnung varchar(80) not null,
    SWS smallint check(SWS >= 0), -- Vorlesung darf keine negative Stunden haben
    Semester smallint check(
        Semester between 1 and 9 -- nur gültige Semestern erlaubt
    ),
    Studiengang Gebiete
)
```

```sql
CREATE TABLE BuchVersionen (
    ISBN      CHAR(10),
    Auflage   SMALLINT       CHECK (Auflage > 0),
    Jahr      INTEGER        CHECK (Jahr BETWEEN 1800 AND 2020),
    Seiten    INTEGER        CHECK (Seiten > 0),
    Preis     DECIMAL(8,2)   CHECK (Preis <= 250),
    PRIMARY KEY (ISBN, Auflage),
    FOREIGN KEY (ISBN) REFERENCES Bücher(ISBN),
    CHECK (
        (SELECT SUM(Preis) FROM BuchVersionen)
        <
        (SELECT SUM(Budget) FROM Lehrstühle)
    )
);
```

### `alter` und `drop table`

>Syntax in SQL-89 (erlaubt nur sehr einfache Änderungen)
-  Neue Spalten werden mit NULL gefüllt, wenn kein Default existiert
```sql
alter table basisrelationenname
	add spaltenname wertebereich
```
Beispiel:
```sql
alter table Lehrstühle
	add Budget decimal(8,2)
```

**Wirkung:**
- Änderung des Relationenschematas
- Erweiterung der existierenden Basisrelation um ein Attribut

>`alter table` in SQL-92
- Angabe von Default-Werten und check-Klauseln erlaubt
```sql
add Budget decimal(8,2) default 10000
	check (Budget > Anzahl Planstellen ∗ 1000)
```

> alter- und drop-Klauseln für Attribute
- Änderung eines Default-Werts:
```sql
alter Spaltenname default_änderung -- nur Änderung von default, nicht von der Datentyp
```
- Löschen einer Spalte
```sql
drop spaltenname [restrict | cascade]
```
	- restrict: Löschen nur wenn keine Abhängigkeiten existieren
	- cascade: Abhängige Objekte werden mitgelöscht

> `drop table`
- restrict und cascade analog zum drop bei Attributen 
	- "analog" bedeutet hier: genauso funktionierend wie bei Attributen
```sql
drop table basisrelationenname
	{ restrict | cascade }
```

> `create index`
- Syntax SQL-89: 
```sql
	create [unique] index indexname on tabelle ( -- unique bedeutet: Keine zwei Zeilen dürfen denselben Wert in diesen Spalten haben
	    spalte1 ordnung,
	    spalte2 ordnung,
    ...
)

)
```
Beispiel:
```sql
create table Bücher (
	ISBN char(10) not null,
	Titel varchar(200),
	Verlagsname varchar(30))
```
- oben fehlt ein *PRIMARY KEY*. Um trotzdem sicherzustellen, dass jede ISBN `einmalig` vorkommt, erstellt man:
```sql
create unique index Buchindex
	on Bücher
	(ISBN asc)
	-- Wirkung von primäre Schlüssel, ohne PRIMARY KEY zu schreiben
```

## Objektrelationale Konzepte

### SQL: 1999
- vielfältige neue Konzepte 
- Unterstützung objektrelationaler Konzepte
	- nutzerdefinierte Datentypen, Vererbung/Spezialisierung, Methoden

>**Neue Datentypen:**

| Datentyp               | Bedeutung                                                 |
| ---------------------- | --------------------------------------------------------- |
| BLOB / CLOB            | Große Binär- bzw. Zeichenkettenobjekte                    |
| Boolean                | Wahrheitswerte: TRUE, FALSE, UNKNOWN                      |
| Array                  | Sammlung mehrerer Werte (mehrwertige Attribute)           |
| REF-Typen              | Identifikation von Tupeln und Navigation per Pfadausdruck |
| ROW-Typen              | Strukturierte Attribute, zusammengesetzte Datensätze      |
| Nutzerdefinierte Typen | Selbst erstellte neue Datentypen                          |
>BLOB und CLOB
- Speicherung großer Zeichenketten/Binärfolgen (Bilder, Audio-/Videodateien, XML-Dokumente)
- Erforderliche Angabe der max. Größe
- Systemspezifische Grenzen
Beispiel:
```sql
create table Proffesoren(
	...
	Foto blob(100K),
);
```
- kein Primär-/Fremdschlüssel, keine Gruppierung/Sortierung

>Kollektionsdatentypen (array)
- Datentypen zur Speicherung mehrerer Werte für ein Attribut
- array-Typkonstruktor
	- Elementdatentyp
	- max. Kardinalität
```sql
-- Syntax
	datentyp array[k]

-- Beispiel
create table Bücher(
	titel varchar(100),
	ISBN varchar(10),
	preis decimal(5, 2),
	abstract clob(10K),
	autoren varchar(20) array[5]
)
```
>Anonyme ROW-Typen
- Typkonstruktor für strukturierte Attribute
- anonym -> kein *expliziter* Typname, nicht wiederverwendbar
- Zweck: Zusammengefasste Attribute wie Adresse, Koordinaten, Zeiträume
```sql
-- Syntax
	row (fname_1, ftyp_1, ...., fname_n, ftyp_n)
	
-- Beispiel
create table Kunden (
	name varchar(50),
	adresse row (strasse var(30), plz char(5), ort varchar(30))
);
```
> Nutzerdefinierte Datentypen (UDT = User defined Types)
- erlaubt, eigene Datentypen zu definieren.
- zwei Arten:
	- *Distinct-Typen:* Umbenennung eines Basistyps, um Strenge Typprüfung zu erzwingen
	- *Strukturierte Typen:* Zusammengesetzte Datentypen mit mehreren Feldern (ähnlich wie bei row, hier aber -> benannt und wiederverwendbar)
>Distinct-Typen
- Umbenennung eines vorhandenen Typs
- erzeugt strenge Typ-Inkompatibilität
- Nur für Attribute, Parameter, Variablen
```sql
-- Beispiel
  create type meter as integer final;
  create type quadratmeter as integer final;

-- Vergleich/Zuweisung zwischen verschiedenen Distinct-Typen nicht erlaubt

--Beispiel Tabelle:
  create table Grundstück (
      laenge meter,
      breite meter,
      flaeche quadratmeter
  );
```
>Strukturierte Typen
- Definition eines komplexen Typs mit mehreren Feldern
- Wiederverwendbar wie eine Klasse
```sql
-- Beispiel
  create type Adresse as (strasse varchar(30),plz char(5),ort varchar(30)) final;

-- Beispiel2
  create type Kunde as (
      knr int,
      name varchar(30),
      lieferadresse Adresse
  ) final;
  
-- Verwendung: Für Attribute, Parameter, Variablen und Tabellen 
```

^7a4f5e

>Subtyping
- Def. von Untertypen als Erweiterung existierender Datentypen (UDTs)
- keine Mehrfachvererbung zulässig
- bei Typdefinition:
	- `not final:`Subtyping zulässig
	- `final:` keine Bildung von Untertypen
```sql
-- Syntax
	untertyp under supertyp
	
-- Beispiel
create type Person as (...) not final
create type Kunde under Person as (...) not final
```

>Strukturierte Typen: Methoden
- SQL-Funktionen/Prozeduren, die UDT zugeordnet sind
- Besonderheiten:
	- impliziter self-Parameter
		- SELF entspricht this in Java oder C++
		- Methoden können auf die Attribute der Instanz zugreifen
	- getrennte Spezifikation 
		- Erst wird die Methode deklarieren, später definieren.

```sql
-- Syntax
create type Kunde as (
	...
) not final
method gesamt bestellsumme() returns decimal(9,2)

create method gesamt bestellsumme() for Kunde
begin
	...
end
```

>Strukturierte Typen für Tabellen
- Def. von Tabellen auf Basis strukturierter Typen  _> Objektrelation
	- Attribute werden Spalten
	- zusätzliche Spalte für OID(benötigt für REF-Typen)

```sql
create table Kunden for Kunde(
	ref is oid uder generated) 
```
-- [[#^7a4f5e|Kunde]] ist Tabellentyp, [[#^7a4f5e|Adresse]] ist Spaltentyp

>Objektidentifikatoren (OID)
- Eindeutige, unveränderliche IDs für Tupel
- Arten: user generated, system generated, aus Attributen abgeleitet

>REF-Typ
- Referenzen auf Objekte (Instanzen strukturierter Typen)
```sql
-- Syntax: 
	ref(Typ) [scope Tabelle]
-- scope → legt fest, auf welche Tabelle die Referenz zeigen darf

-- Beispiel
create type Person as (
	personalnr integer,
	name varchar(30),
	vorgesetzter ref(Person) )
create table Angestellte of Person ( ... )
create table Abteilungen (
	name varchar(10),
	leiter ref(Person) scope(Angestellte),
	mitarbeiter ref(Person)
		scope(Angestellte) array[10] )
```
- Person-Objekte können auf andere Person-Objekte verweisen (vorgesetzter)
- Tabellen können REF-Felder enthalten, z. B. leiter ref(Person) scope(Angestellte)

**Systemumsetzung**
- Objekt-relationale Features sind systemabhängig und oft inkompatibel
- Nicht überall vollständig unterstützt → Migration schwierig
