#DB2 #DB2Kapitel1 #DB2Kapitel2 #DB2Kapitel3 

![[Pasted image 20251117164646.png]]

Antwort:

| $F_1 : F_2$ | $[min_1, max_1]$ | $[min_2, max_2]$ |
| ----------- | ---------------- | ---------------- |
| 1 : 1       | [0,1]            | [0,1]            |
| 1 : N       | [0,n]            | [0,1]            |
| N : M       | [0,n]            | [0,n]            |

![[Pasted image 20251117171504.png]]

![[Pasted image 20251117171546.png]]
Antwort:
	Bühne(<u>BühnenNr</u>; Anzahl Plätze),
	Aufführung(<u>A.ID</u>, Uhrzeit, Datum),
	Theaterstück(<u>T.ID</u>, Titel, Genre),
	Mitarbeiter:in(<u>M.ID</u>, Name, Gehalt),
	Schauspieler:in(<span style='text-decoration: overline;'><u>M.ID</u></span>, Größe),
	Regisseur:in(<u><span style='text-decoration: overline;'>M.ID</span></u>, Ausbildung),
	ist auf(<u><span style='text-decoration: overline;'>A.ID</span></u>, <span style='text-decoration: overline;'>BühnenNr</span>), 
	gehört zu(<u><span style='text-decoration: overline;'>A.ID</span></u>, <span style='text-decoration: overline;'>T.ID</span>),
	spielt in(<u><span style='text-decoration: overline;'>M.ID</span>, <span style='text-decoration: overline;'>T.ID</span></u>, Rollenname)
	leitet(<u><span style='text-decoration: overline;'>M.ID</span>, <span style='text-decoration: overline;'>T.ID</span></u>)

![[Pasted image 20251117175452.png]]
Antwort:
leitet mit Regisseur und Theaterstück,
gehört zu mit Theaterstück und Aufführung
ist auf mit Bühne und aufführung

![[Pasted image 20251117175603.png]]
	Aufführung(<u>A.ID</u>, Uhrzeit, Datum, <span style='text-decoration: overline;'>BühnenNr</span>, <span style='text-decoration: overline;'>T.ID</span>),
	Theaterstück(<u>T.ID</u>, Titel, Genre, <span style='text-decoration: overline;'>M.ID</span>)

![[Pasted image 20251117183300.png]]
Antwort
FWFWF

![[Pasted image 20251117183414.png]]

```sql
-- Antwort:
create table Product(
	prodict_id INT primary key,
	name varchar(30) not null,
	internal_name char(200),
	price decimal(6,2) check(price >= 50)
);
```

![[Pasted image 20251117183724.png]]

```sql
 insert into Product(product_id, internal_name, price) values(100, 'Ram2000', 100,20);
```
![[Pasted image 20251117184507.png]]

