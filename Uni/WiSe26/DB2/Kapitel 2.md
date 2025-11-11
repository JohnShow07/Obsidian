#DB2 #Kapitel2
# Datenbanken: eine Einführung - Wiederholung
## Entity- Relationship-Modell

**Entity:** Objeklte, in drin Infos gespeichert werden
**Relationships:** Beziehung zwischen Entitys
**Attribut:** Eigenschaften von Entitys oder Relationships
$\mu(E)$ =  Menge der Möglichen Entities vom Typ E
$\sigma(E)$ = Menge der aktuellen Entites vom Typ E ein einem Zustand $\sigma$
![[Pasted image 20251111143239.png]]
![[Pasted image 20251111143316.png]]
![[Pasted image 20251111143333.png]]
![[Pasted image 20251111143346.png]]
### Funktionale Beziehungen
Textuell: $R: E_1 \rightarrow E_2$ 
Graphisch: 
![[Pasted image 20251111143638.png]]
Bedeutung: $\sigma(R) : \sigma(E_1) \rightarrow \sigma(E_2)$ 

**Schlüsselattribute:** spezielle Attribute, wird durch Unterstreichen markiert
**Die IST-Beziehung:** Jede Entity A ist genau einer Entity B zugeordnet, nicht andersrum
![[Pasted image 20251111144725.png]]
### Kardinalitäten
- **Notation:** $R(E_1,....,E_i[min_i,max_j],....,E_n)$ 
- Wertangabe * heißt **beliebig.**
- 