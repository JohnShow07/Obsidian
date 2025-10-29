# C-Sicherheitsprobleme


POSIX: Portable Operating System Interface
- definiert grundlegende Systemaufrufe

Die kommerzielle Betriebssysteme, alle, in C geschrieben
- viele Programme portierbar und C-Code ist sehr schnell
- C++ war langsamer -> nicht verwendet
Probleme:
- Manuelles Speicher- und Zeiger Management _> führt zu Memory leaks(vergessene Freigaben) und Dangling Pointers(Zeiger ins Nichts)
- Keine Überprüfung, ob ein Zeiger gültig ist _>Use-after-free (Zugriff auf bereits freigegebenen Speicher), Zugriff über Nullzeiger_
- Keine Bounds-Check bei Array _> Buffer Overflows
- Fehlende Typsicherheit _> void oder Casts können beliebig zwischen Typen umgewandelt werden, Compiler kann dadurch viele Fehler nicht verhindern. In Rust akzeptiert der Compiler diese Fehlern nicht, gibt immer Warnung aus._
Warum noch C:
- schnell
- nützliche Flexibilität(wegen Zeigerarithmetik und Typumwandlungen)
- Falls alles richtig, manuelle Speicherverwaltung schneller als automatische(wie bei Java)