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

```sql
create domain Gebiete varchar(20)
	default ’Informatik’
```
