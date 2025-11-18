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

```sql
 select name, internal_name from Product where preis < 99,20;
```

![[Pasted image 20251117184637.png]]

```sql
 alter table Product 
 add received_on date, default '2000-01-01';
```

![[Pasted image 20251117184938.png]]

```sql
 alter table Product 
 add purchased_price decimal(6, 2), check(purchase_price > 0);
```


***GPT-Fragen*** 

**1.** Gegeben ist eine Beziehung _„verwaltet“_ zwischen _Abteilung_ und _Projekt_ mit Kardinalitäten:  
Abteilung [1,1] — verwaltet — Projekt [0,*].  
Ist diese Beziehung **funktional**, **partiell funktional**, oder keine funktionale Beziehung? Begründe.

Antwort: 

Abteilung [1,1] — verwaltet ----[0,*]--- Projekt .  

Richtung 1 -> N : Eine Abteilung kann mehrere Projekte haben : nicht funktional
Richtung N -> 1: Ein Projekt muss min und max nur eine Abteilung haben: funktional, nicht partiell

**2.** Ein ER-Diagramm zeigt eine Beziehung _„besitzt“_ zwischen _Kunde_ und _Auto_, Kardinalität Kunde [0,*], Auto [1,1].  
Formuliere die Bedeutung dieser Kardinalitäten in eigenen Worten.

**1.** Gib zu jedem der vier ACID-Begriffe eine korrekte Definition in _einem kurzen Satz_.


**2.** Welche Art von Problem tritt bei folgendem Ablauf auf?

T1: read(X)  
T2: read(X)  
T2: X := X – 5  
T2: write(X)  
T1: X := X + 5  
T1: write(X)

A) Dirty Read  
B) Lost Update  
C) Nonrepeatable Read  
D) Phantom

**3.** Was unterscheidet „repeatable read“ von „serializable“?

**4.** Nenne den Inhalt eines **Wartegraphen**. (Welche Knoten? Welche Kanten?)

**5.** Warum verhindert _strict 2PL_ kaskadierende Abbrüche?

**6.** Welches Sperrprotokoll garantiert keine Deadlocks? Begründe.

**7.** Ein Schedule enthält einen Zyklus im Konfliktgraphen.  
Ist er serialisierbar? Begründe.