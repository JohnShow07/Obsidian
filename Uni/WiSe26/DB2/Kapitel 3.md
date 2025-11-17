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
	- restrict