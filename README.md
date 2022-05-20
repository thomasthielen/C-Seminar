# C/C++ Seminar

Neben den Basics des Programmaufbaus und der Kompilierung möchte ich in diesem Guide primär Standardlösungen für übliche Problemstellungen sammeln.

Diese entnehme ich den diversen Tutorien, Projekten und Altklausuren unseres Seminars.

Zusätzlich werde ich auch auf übliche Fehler oder Error Messages (und wie man diese beheben kann) eingehen.

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

## Eingabe & Ausgabe

[Dokumentation zu printf ()](https://www.cplusplus.com/reference/cstdio/printf/)

[Dokumentation zu scanf ()](https://www.cplusplus.com/reference/cstdio/scanf/)

## Pointer (& und \*\)

[Exzellenter Guide auf cplusplus.com](https://www.cplusplus.com/doc/tutorial/pointers/)

### Wichtige Auszüge

& is the address-of operator, and can be read simply as "address of"

\* is the dereference operator, and can be read as "value pointed to by"
