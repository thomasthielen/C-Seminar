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

## Woche 1

### Eingabe & Ausgabe

[Dokumentation zu printf ()](https://www.cplusplus.com/reference/cstdio/printf/)

Allgemein:

```c++
printf ("numbers: %.2f %d %s \n", 3.1416, 3, '3.1416');  // numbers: 3.14 3 3.1416
```

[Dokumentation zu scanf ()](https://www.cplusplus.com/reference/cstdio/scanf/)

```c++
scanf ("%d.%d.%d", &day, &month, &year);  
// Wenn wir 13.09:1999 eingeben: day = 13, month = 9, year bleibt unverändert (fehlerhafte Eingabe mit :)
```

### Pointer

[Aus dem exzellenten Guide auf cplusplus.com](https://www.cplusplus.com/doc/tutorial/pointers/)

#### Pointer und Referenzen

& is the address-of operator, and can be read simply as "address of"

\* is the dereference operator, and can be read as "value pointed to by"

```c++
  int firstvalue = 5, secondvalue = 15;
  int * p1, * p2;

  p1 = &firstvalue;  // p1 = address of firstvalue
  p2 = &secondvalue; // p2 = address of secondvalue
  *p1 = 10;          // value pointed to by p1 = 10
  *p2 = *p1;         // value pointed to by p2 = value pointed to by p1
  p1 = p2;           // p1 = p2 (value of pointer is copied)
  *p1 = 20;          // value pointed to by p1 = 20
  
  // firstvalue = 10
  // secondvalue = 20
```

Referenzen auf Pointer schreiben sich wie folgt:

```c++
int i = 0;
int *p = &i;
int *&rp = p;
```

Wir haben folgende Möglichkeiten Pointer & Referenzen auszugeben:

```c++
int number = 1;

// 3 Möglichkeiten für Pointer:
int *p = &number;
printf ("p=%p    \n", p);     // Adresse von number
printf ("&p=%p \n", &p);      // Adresse von p
printf ("*p=%d  \n", *p);     // Wert von number

// 2 Möglichkeiten für Referenzen:
int &r = number;
printf ("r=%d    \n", r);     // Wert von number
printf ("&r=%d \n", &r);      // Adresse von number
```

#### Pointer und Arrays

Jedes Array entspricht einem Pointer des entsprechenden Datentyps auf das erste Element.

Aus dem folgenden Code lässt sich weiteres schließen:

```c++
// more pointers
#include <iostream>
using namespace std;

int main ()
{
  int numbers[5];
  int * p;
  p = numbers;  *p = 10;      // p wird als Poiner auf die Adresse des ersten Element in numbers[] gesetzt (numbers[0])
  p++;  *p = 20;              // p wird um 1 erhöht und zeigt damit auf numbers[1]
  p = &numbers[2];  *p = 30;  // p zeigt auf die Adresse von numbers[2]
  p = numbers + 3;  *p = 40;  // p zeigt auf die Adresse von numbers[0] + ein offset von 3, was zu numbers[3] führt
  p = numbers;  *(p+4) = 50;  // hier wird temporär und lokal über die Adresse von p+4 numbers[4] geändert
  for (int n=0; n<5; n++)
    cout << numbers[n] << ", ";
  return 0;
}
```

Wichtig: Ein Array selbst besitzt nur Pointer auf Datenbereiche mit den jeweiligen Datentypen.

D.h. dass `&numbers[0] = 74` und danach `&numbers[1] = 75`.

Deshalb können wir den Pointer um 1 erhöhen um auf das nächste Element des Arrays zugreifen zu können.
