#DB2 #Kapitel2
# Datenbanken: eine Einführung - Wiederholung
## Entity- Relationship-Modell

**Entity:** Objeklte, in drin Infos gespeichert werden
**Relationships:** Beziehung zwischen Entitys
**Attribut:** Eigenschaften von Entitys oder Relationships
$\mu(E)$ =  Menge der Möglichen Entities vom Typ E
$\sigma(E)$ = Menge der aktuellen Entites vom Typ E ein einem Zustand $\sigma$
**Semantik einer Attributdeklaration:** $\sigma(A) : \sigma(E) \rightarrow \sigma(D)$ 
![[Pasted image 20251111143239.png]]
![[Pasted image 20251111143316.png]]
![[Pasted image 20251111143333.png]]
![[Pasted image 20251111143346.png]]
### Funktionale Beziehungen
Textuell: $R: E_1 \rightarrow E_2$  
- Jede Entität aus **E₁** (links)  **hat genau eine** zugehörige Entität aus **E₂** (rechts).
- Beziehung ist also **funktional** = eindeutig.
Graphisch: 
![[Pasted image 20251111143638.png]]
Bedeutung: $\sigma(R) : \sigma(E_1) \rightarrow \sigma(E_2)$ 
Das heißt:
- Für **jeden** Professor gibt es **genau ein** Zimmer, in dem er sitzt.

**Schlüsselattribute:** spezielle Attribute, wird durch Unterstreichen markiert
### Die IST-Beziehung 
>Jede Entity A ist genau einer Entity B zugeordnet, nicht andersrum
![[Pasted image 20251111144725.png]]
### Kardinalitäten
- **Notation:** $R(E_1,....,E_i[min_i,max_j],....,E_n)$ 
- Wertangabe * heißt **beliebig.**
- $[0,*]$ ist **Standartannahme**
- $RE_1[0,1],E_2)$ beschreibt eine (partielle) funktionale Beziehung $R: E_1 \rightarrow E_2$ 
	- Da jede Instanz aus $E_1$ maximal einer Instanz aus $E_2$ zugeordnet ist.
- $RE_1[1,1],E_2)$ beschreibt eine totale funktionale Beziehung
#### Beispiele
```
arbeitet_in(Mitarbeiter[0,1], Raum[0,3])
```
- Jedem Mitarbeiter ein Raum, aber einige Mitarbeiter kein Raum
- Pro Zimmer max 3 Mitarbeiter
```
verantwortlich(Mitarbeiter[0,*], Rechner[1,1])
```
- Jedem Rechner genau ein Mitarbeiter

## Das Relationenmodell

![[Pasted image 20251111152752.png]]
![[Pasted image 20251111152944.png]]

| **Begriff**             | **Informale Bedeutung**                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------- |
| Attribut                | Spalte einer Tabelle                                                                                    |
| Wertebereich            | mögliche Werte eines Attributs (auch Domäne)                                                            |
| Attributwert            | Element eines Wertebereichs                                                                             |
| Relationenschema        | Menge von Attributen                                                                                    |
| Relation                | Menge von Zeilen einer Tabelle                                                                          |
| Tupel                   | Zeile einer Tabelle                                                                                     |
| Datenbankschema         | Menge von Relationenschemata                                                                            |
| Datenbank               | Menge von Relationen (Basisrelationen)                                                                  |
| Schlüssel               | minimale Menge von Attributen, deren Werte ein Tupel einer Tabelle eindeutig identifizieren             |
| Primärschlüssel         | ein beim Datenbankentwurf ausgezeichneter<br>Schlüssel                                                  |
| Fremdschlüssel          | Attributmenge, die in einer anderen Relation<br>Schlüssel ist                                           |
| Fremdschlüsselbedingung | alle Attributwerte des Fremdschlüssels tauchen in der<br>anderen Relation als Werte des Schl¨ussels auf |
### Integritätsbedingungen

- **identifizierende Attributmenge:** Eine Tabelle muss etwas haben, womit man eden Datensatz eindeutig unterscheiden kann.
- Wenn du **zwei Datensätze hast, die nicht identisch sind**, müssen sie sich **mindestens in einem Attribut** aus dieser Menge unterscheiden.
- Der Schlüssel muss eindeutig sein. Keine zwei Zeilen dürfen denselben Schlüssel haben.
- Eine Tabelle kann mehrere mögliche Schlüssel haben (Kandidaten), aber **nur EIN Primärschlüssel** wird gewählt.

### von ER-Diagramm ins Relationmodell
- *kapazität:* Anzahl der möglichen Tupel, die durch die Kardinalitäten des ER-Modells erlaubt sind. 
- **Kapazitätserhaltend:** Relation erlaubt exakt die Tupel, die laut ER-Modell zulässig sind.
- **Kapazitätserhöhend:** Die Tabelle mehr Tupel erzeugt als sinnvoll möglich (typisch bei n:m oder 1:n)
![[Pasted image 20251115174059.png]]
- **Kapazitätsvermindernd:** Die Tabelle weniger Tupel abbildet als erlaubt wäre (typisch bei 1.1)
![[Pasted image 20251115174253.png]]

##### Regeln bei der Abbildung auf das relationale Modell:
- Entity-Typen und Beziehungstypen → Relationenschemata
	- Attribute → Attribute des Relationenschemas
	- Schlüssel werden übernommen
- Kardinalitäten der Beziehungen → Wahl der Schlüssel
- Relationenschemata von Entity- und Beziehungstypen können eventuell miteinander verschmolzen werden
- Einführung diverser Fremdschlüsselbedingungen ![[Pasted image 20251116130414.png]]
##### Abbildung von Beziehungstypen
- Ein Beziehungstyp wird zu einem Relationenschema mit:
	- allen Attributen des Beziehungstyps
	- den Primärschlüsseln der beteiligten Entity-Typen
**Schlüsselauswahl (für binäre Beziehungen):**
- m:n-Beziehung:
	- Beide Primärschlüssel der Entity-Typen werden Schlüsselattribute.
	- Zusammen bilden sie den Primärschlüssel der Relation.
- 1:n-Beziehung:
	- Der Primärschlüssel der n-Seite wird Primärschlüssel der Relation.
	- (Bei funktionaler Notation: die Seite ohne Pfeilspitze.)
- 1:1-Beziehung:
	- Beide Primärschlüssel der Entity-Typen werden Schlüsselattribute.
	- Einer davon wird als Primärschlüssel ausgewählt.
Hinweis:  
Diese Regeln gelten auch bei optionalen **Beziehungen ([0, ]).**
**bei zwingenden Beziehungen ([1, ]):**
- 1:n-Beziehung:
	- Das Entity-Relationenschema der n-Seite kann in das Relationenschema der Beziehung integriert werden.
- 1:1-Beziehung:
	- Beide Entity-Relationenschemata können in das Relationenschema der Beziehung integriert werden.