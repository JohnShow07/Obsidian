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

```
create table basisrelationenname (
	spaltenname 1 wertebereich 1 [not null],
	...
	spaltenname k wertebereich k [not null])
```

> **Datentypen**
- 