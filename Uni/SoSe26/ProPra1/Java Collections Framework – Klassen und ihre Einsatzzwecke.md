## Interfaces (Überblick)

| Interface               | Zweck                                                                                |
| ----------------------- | ------------------------------------------------------------------------------------ |
| **Iterable**            | Basis für alles, was iteriert werden kann (z. B. mit `for-each`).                    |
| **Collection**          | Allgemeine Sammlung von Elementen.                                                   |
| **Set**                 | Menge ohne Duplikate.                                                                |
| **SequencedCollection** | Sammlung mit definierter Reihenfolge (Java 21+); Zugriff auf erstes/letztes Element. |
| **Queue**               | Warteschlange (FIFO-Prinzip).                                                        |
| **List**                | Geordnete Sequenz mit Indexzugriff, Duplikate erlaubt.                               |
| **Deque**               | Doppelseitige Queue (Einfügen/Entnehmen an beiden Enden).                            |
| **SequencedSet**        | Set mit definierter Reihenfolge.                                                     |
| **SortedSet**           | Set, dessen Elemente sortiert sind.                                                  |
| **Map**                 | Schlüssel-Wert-Zuordnung (keine Collection!).                                        |
| **SequencedMap**        | Map mit definierter Reihenfolge.                                                     |
| **SortedMap**           | Map, deren Schlüssel sortiert sind.                                                  |

---

## Konkrete Klassen – Wann verwende ich was?

### Listen (`List`)

- **ArrayList**  
    Intern dynamisches Array.  
    ✅ Schneller wahlfreier Zugriff per Index `get(i)` (O(1)).  
    ✅ Beste Standardwahl für Listen.  
    ❌ Einfügen/Löschen in der Mitte ist langsam (O(n)).  
    → **Einsatz:** Wenn vor allem gelesen oder hinten angefügt wird.
    
- **LinkedList**  
    Doppelt verkettete Liste; implementiert auch `Deque`.  
    ✅ Effizientes Einfügen/Löschen am Anfang/Ende (O(1)).  
    ❌ Indexzugriff teuer (O(n)).  
    → **Einsatz:** Als Queue/Deque oder wenn ständig vorne/hinten eingefügt/entfernt wird.
    

### Mengen (`Set`)

- **HashSet**  
    Hash-basiert, keine garantierte Reihenfolge.  
    ✅ Sehr schnelles `add`, `remove`, `contains` (O(1) im Durchschnitt).  
    → **Einsatz:** Standardmenge ohne Duplikate, wenn Reihenfolge egal ist.
    
- **LinkedHashSet**  
    Hash-Set mit zusätzlicher Verkettung in Einfügereihenfolge.  
    → **Einsatz:** Wenn Eindeutigkeit wichtig ist und Einfügereihenfolge erhalten bleiben soll.
    
- **TreeSet** (implementiert `SortedSet`)  
    Rot-Schwarz-Baum, sortierte Speicherung.  
    ✅ Operationen O(log n), sortierte Iteration.  
    → **Einsatz:** Wenn Elemente automatisch sortiert sein sollen oder Bereichsabfragen (z. B. `subSet`) gebraucht werden.
    

### Maps (`Map`)

- **HashMap**  
    Standard-Map auf Hash-Basis, ungeordnet.  
    ✅ Schnell (O(1) im Schnitt).  
    → **Einsatz:** Standardwahl für Schlüssel-Wert-Speicherung.
    
- **LinkedHashMap**  
    HashMap + Einfügereihenfolge (oder Zugriffsreihenfolge).  
    → **Einsatz:** Wenn Iterationsreihenfolge vorhersagbar sein soll, z. B. für LRU-Caches.
    
- **TreeMap** (implementiert `SortedMap`)  
    Sortierte Map (Rot-Schwarz-Baum).  
    → **Einsatz:** Wenn Schlüssel sortiert iteriert oder Bereichsabfragen gemacht werden sollen.
    

### Abstrakte Klasse

- **AbstractList**  
    Basisklasse für eigene Listen-Implementierungen; nicht direkt instanziierbar.

---

## Faustregeln für die Auswahl

|Anforderung|Empfehlung|
|---|---|
|Liste mit häufigem Indexzugriff|**ArrayList**|
|Häufiges Einfügen/Löschen vorne/hinten|**LinkedList** / **ArrayDeque**|
|Eindeutige Elemente, Reihenfolge egal|**HashSet**|
|Eindeutige Elemente, Einfügereihenfolge|**LinkedHashSet**|
|Eindeutige Elemente, sortiert|**TreeSet**|
|Key-Value, Reihenfolge egal|**HashMap**|
|Key-Value, Einfügereihenfolge|**LinkedHashMap**|
|Key-Value, nach Schlüssel sortiert|**TreeMap**|