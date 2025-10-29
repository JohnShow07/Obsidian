# C-Sicherheitsprobleme


POSIX: Protable Operating System Interface
- definiert grundlegende Systemaufrufe

Die kommerzielle Betriebssystemme, alle, in C geschrieben
- viele Programme portierbar und C-Code ist sehr schnell
- C++ war langsamer -> nicht verwendet
Probleme:
- Manuelles Speicher- und Zeiger Management _> führt zu Memory leaks(vergessene Freigaben) und Dangling Pointers(Zeiger ins Nichts)
- Keine Überprufung, ob ein Zeiger g