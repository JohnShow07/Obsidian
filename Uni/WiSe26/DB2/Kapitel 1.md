#DB2 #DB2Kapitel1
## Section 1: Transaktionen

-> Transaktion ist eine Folge von Operationen, die die DB von konsistenten zum konsistenten, veränderten Zustand überführt. (Dabei ACID eingehalten)

#### Beispiele: 
	- Platzreservierung für Flüge aus vielen Reisebüros gleichzeitig
	- überschneidende Kontooperationen einer Bank.

#### Aspekte:
- **semantische Integrität**: korrekter und konsistenter DB Zustand nach Ende der Transaktion.
- **Ablaufintegrität**: Fehler durch "gleichzeitigen" Zugriff mehrerer Benutzer auf dieselben Daten vermeiden

### ACID

**A**tomicity: Transaktion wird entweder ganz oder gar nicht ausgeführt
**C**onsistency: DB vor und nach Transaktion in konsistenter Zustand
**I**solation: User soll den Eindruck haben, dass er alleine an der DB arbeitet.
**D**urability: nach erfolgreichen Abschluss einer Transaktion muss das Ergebnis dauerhaft in DB gespeichert werden.

#### Commands zur Steuerung
	BOT: Beginning-of-Transaction
	commit: die Transaktion soll erfolgreich beendet werden
	abort: die Transaktion doll abgebrochen werden

##### Ein Beispiel:
- Übertragung eines Betrages B von einem Konto K1 auf ein anderes Konto K2 (Bedingung: die Summe der Kontostände bleiben erhalten)
- vereinfachte Notation: 
```
Transfer = < K1:=K1-B; K2:=K2+B >;
```
- Realisierung in SQL: als Sequenz zweier Elementarer Änderungen Bsp:
```
T_1: read(A,x); x:=x-200; write(x, A);
T_2: read(B,y); y:=y+200; write(y, B);
```
- **Falls ein Systemfehlet/Abbruch beim Transaktion passiert, ist DB inkonsistent.**


### Mehrbenutzerbetrieb
-> Mehrere Nutzer arbeiten gleichzeitig mit derselben DB.

#### 1.2) Probleme:
- **nonrepeatable Read**: Änderung eines Wertes zwischen zwei Lesevorgängen
	- BSP: A wird von X gelesen. Dann im Laufe des T_2 wird A zu A/2. X ist trotz dieser Änderung A geblieben.
	![[Pasted image 20251019222730.png]]
- **Dirty Read:** eine T liest Daten, die eine andere noch nicht bestätigt(commit) hat.
	- BSP: T_1 ändert X um 100 und speichert diese. T_2 liest die neue X und berechnet -> commited. Jedoch T_1 war noch nicht fertig/commited, also kann nich abgebrochen werden. Genau dies passiert und werden alle Änderungen in T_1 Rückgängig gemacht. T_2 wird jedoch die gelöschte Änderungen beibehalten.
	![[Pasted image 20251019223203.png]]
	- **the Phantom-Problem:** Eine T liest mehrere Daten (zB mit SELECT) während eine andere T neue Daten hinzufügt oder löscht. 
	  -> also neue Datensätze(Phantom) durch das Lesen der anderer T, den man noch nie gesehen hat. Unten zählt T_1 die Mitarbeiter und speichert in X, dann möchte jedem eine Bonus geben. Währenddessen fügt T_2 eine neue Mitarbeiter, welcher leider nicht diesen Bonus kriegt, da T_1 bereits die Mitarbeiter gezählt hat. 
	![[Pasted image 20251025174246.png]]
	- **lost Update:** Zwei Ts lesen und ändern denselben wert gleichzeitig und eine Änderung(die Erste) geht verloren. Im Bsp möchten beide Ts die X um 1 erhöhen. T_2 überschreibt die X-Änderung von T_1, da die Änderung noch nicht gespeichert wurde, daher bleibt der Wert bei 11, obwohl das Ziel 12 war. 
	![[Pasted image 20251025174713.png]]

### 1.3) Serialisierbarkeit
-> bedeutet, dass mehrere gleichzeitig laufende Ts so ausgeführt werden, als wären sie nacheinander(seriell) ausgeführt worden.
-> Das Ergebnis ist dasselbe, als hätte man die Ts nacheinander ausgeführt

**Bsp:**
	$T_1$: readA; A:= A - 10; writeA; readB;
		B := B + 10; writeB;
	$T_2$: readB; B:= B - 20; writeB; readC;
		C := C + 20; writeC;

**Bspiele für verschränkte Ausführungen:**
-> diese bedeutet, dass mehrere Transaktionen gleichzeitig laufen, und ihre Befehle sich abwechseln (also ineinander verschränkt sind).
![[Pasted image 20251025182814.png]]
![[Pasted image 20251026210217.png]]
-> Eine verschränkte Ausführung mehrerer Ts heißt **serialisierbar**, wenn ihr Effekt identisch zum Effekt einer (beliebig gewählten) seriellen Ausführung dieser Ts ist.

**Das Read/Write-Modell:** Eine T besteht aus eine endliche Folge von Operationen(Schritten) $p_i$ der form $r(x_i)$ oder $w(x_i)$:

- $T = p_1p_2p_3...p_n$ mit $p_i \in {r(x_i),w(x_i)}$ 

**Eine vollständige T** hat als letzten Schritt entweder Abbruch a oder Commit c: 
$T = p_1...p_n a$   oder   $T = p_1...p_n c$ 

**Eine Schedule** ist ein Präfix eines vollständigen Schedules
**Eine vollständige Schedule** ist eine Folge von DB-Operationen.
- alle OPs gehören zu vollständigen Transaktion
- alle OPS dieser Ts treten im Schedule in derselben relativen Reihenfolge wie in der T auf.
![[Pasted image 20251026215541.png]]

Ein serieller Schedule s für T ist ein vollständiger Schedule in der folgenden Form: 
![[Pasted image 20251026221603.png]]
_> Ein Schedule ist korrekt, wenn das Ergebnis genauso ist, wie bei einer seriellen Ausführung der Ts._

- Ein **Schedule** ist eine **Reihenfolge**, in der Befehle (read/write) von mehreren Transaktionen ausgeführt werden.
    
- Wenn das Ergebnis am Ende **so aussieht**, als hätte man **die Transaktionen nacheinander (seriell)** ausgeführt,  
    → dann ist dieser Schedule **korrekt / serialisierbar**.

Konflikte: