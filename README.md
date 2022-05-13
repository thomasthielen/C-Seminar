# C/C++ Seminar

## Basics

### Kompilierung

Terminal in entsprechendem Folder öffnen

C Compiler: "gcc"

C++ Compiler: "g++"

```text
g++ [options] -o a1 a1.cc
```

Einige Optionen:

- `-o name` benennt das kompilierte Programm in "name" um
- `-Wall` wichtige Compiler Warnungen
- `-std=xxx` z.B. _-std=c99_, C/C++ Erweiterungen
- `-O[0|1|2|3|s]` z.B. _-O3_, optimiert den Code in versch. Granularität 
- `-l lib` z.B. _-l m_, fügt dem Linker eine zusätzliche Bibliothek hinzu (hier: Math-Library)

### Programme / Header

Header in C: (man beachte das .h)

```c
#include <stdio.h>

int main ()
{
  return 0;
}
```

Header in C++:
```c++
#include <iostream>

int main ()
{
  return 0;
}
```

Ohne eine `int main()` Funktion beschwert sich der Linker

Das `return 0` wird bei der main-Funktion automatisch ergänzt
