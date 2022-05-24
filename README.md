# C/C++ Seminar

Neben den Basics des Programmaufbaus und der Kompilierung möchte ich in diesem Guide primär Standardlösungen für übliche Problemstellungen sammeln.

Diese entnehme ich den diversen Tutorien, Projekten und Altklausuren unseres Seminars.

Zusätzlich werde ich auch auf übliche Fehler oder Error Messages (und wie man diese beheben kann) eingehen.

# Basics

## Kompilierung

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

## Programme / Header

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

# Woche 1

## Eingabe & Ausgabe

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

## Pointer

[Aus dem exzellenten Guide auf cplusplus.com](https://www.cplusplus.com/doc/tutorial/pointers/)

### Pointer und Referenzen

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
printf ("p=%p   ", p);    // Adresse von number
printf ("&p=%p  ", &p);   // Adresse von p
printf ("*p=%d  ", *p);   // Wert von number

// 2 Möglichkeiten für Referenzen:
int &r = number;
printf ("r=%d   ", r);    // Wert von number
printf ("&r=%d  ", &r);   // Adresse von number
```

### Pointer und Arrays

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

## const & constexpr

- const: der Wert verändert sich nicht (mehr)
- constexpr: der Wert verändert sich nicht (mehr) UND der Compiler kann den Wert der Variable nachvollziehen
  - z.B. weil dieser nur mit Konstanten oder const Variablen initialisiert wird

Syntax:

```c++
int i = 0;
constexpr int j = 4;
constexpr int k = j + 2;
const int l = i + j;      // der Compiler kann nicht feststellen, ob i konstant bleibt
```

Bei Pointern kann const zweifach angewendet werden: Für den Wert auf den der Pointer zeigt und/oder den Pointer selbst:

1. Der Wert des Pointers:

```c++
int i = 0;
const int j = 2;
const int *p = &i;
*p = 3;       // illegal!
int *p2 = &j; // illegal!
```

  - Man bemerke: Die Variable selbst muss nicht const sein, der Pointer stellt einfach nur sicher dass durch _ihn_ der Wert der Variable nicht verändert wird.

  - Umgekehrt **muss** für eine const Variable der Pointer ebenfalls const sein!

2. Der Pointer selbst:

```c++
int i = 0;
int j = 3;
int *const p = &i;
int *p2 = &j;
p = p2;     // illegal!
```

# Woche 2

## namespaces

[Tutorial](http://cplusplus.com/doc/tutorial/namespaces/)

Syntax:

```c++
namespace ns1
{
  int i;
  namespace ns2
  {
    int j;
  }
}

int main ()
{
  using ns1::ns2;   // Wir nutzen dadurch erstmal NUR ns2
  ns1::i = 0;
  j = 3;
}
```

Auf globale Variablen kann auch wie folgt zugegriffen werden:

```c++
int d;

int main ()
{
  ::d = 3;
}
```

Dies ist aber redundant, da wir auch mit `d = 3;` auf die globale Variable zugreifen hätten können.

Kommen in mehreren namespaces die wir mit `using` referenzieren, eine Variable mit dem gleichen Namen vor, so erhalten wir einen `error: reference to 'x' is ambiguous` Error.

## Bitwise Operators

| operator | logical equivalent |	description          |
| -        | -                  | -                    |
| & 	     | AND	              | Bitwise AND          |
| \|	     | OR                 | Bitwise inclusive OR |
| ^	       | XOR	              | Bitwise exclusive OR |
| ~	       | NOT	              | Bit inversion        |
| <<	     | SHL	              | Shift bits left      |
| >>	     | SHR	              | Shift bits right     |

Beispiele:

```c++
  unsigned char z = 0b10101010;   // diese Zuweisung erfolgt nach jedem Beispiel

// bestimmte Bits setzen
  z = z | 0b1;    // das letzte Bit setzen => 10101011
  
// bestimmte Bits löschen
  z = z & ~0b010  // das zweitletzte Bit löschen => 10101000
  
// bestimmte Bits invertieren
  z = z ^ 0b100;  // das drittletzte Bit invertieren => 10101110
  
// alle außer einem Bit setzen
  z = z | ~0b1;   // alle außer dem letzten Bit werden gesetzt => 11111110
  
// alle außer einem Bit löschen
  z = z & 0b10;   // alle außer dem vorletzten Bit werden gelöscht => 00000010
  
// alle außer einem Bit invertieren
  z = z ^ ~0b100  // alle außer dem drittletzten Bit werden invertiert => 01010001
  
// Bits untereinander vertauschen
  z = (z >> 1 & 0b10) | (z << 1 & 0b100) | (z & ~0b110);  // das vorletzte und drittletzte Bit werden vertauscht => 10101100
  
// zyklische Rotation von Bits
  int bits = CHAR_BIT;  // = 8
  z = (z >> k) | (z << bits >> k);
```

## Simple Eingaben mit cin

(aus P2/2)

```c++
  double temp;
  char symbol;

  cout << "Temperatur eingeben: ";
  cin >> temp >> symbol;
  bool fahrenheitToCelcius = symbol == 'F' || symbol == 'f';
  bool celciusToFahrenheit = symbol == 'C' || symbol == 'c';
```

## bitset zur Ausgabe von Bitcode

(aus P2/3)

Zur bytewise Ausgabe von Variablen aller Datentypen

```c++
  char z = 'a';
  cout << bitset<8> (z);  // gibt 'a' bytewise mit 8 Stellen aus (01100001)
```

# Woche 3

## Header

In einer Header-Datei "a2.h" zu dem Programm "a2.cc" müssen Dinge deklariert oder aufgerufen werden, die in a2.cc selbst nicht deklariert/aufgerufen werden.

`#include "a2.h"`

Dazu können folgende Dinge gehören:

- Funktionen, die vor ihrer Definition in a2.cc aufgerufen werden
- `#include`-Anweisungen aus der Standardbibliothek die z.B. für std::cout benötigt werden (`#include <iostream>`)
- `typedef`-Deklarationen für einen Alias eines Datentyps, der in a2.cc genutzt aber nicht deklariert wird
- globale Variablen die in a2.cc genutzt werden, aber dort nicht deklariert werden

```c++
  #include <iostream>

  typedef unsigned int uint;

  uint y;

  uint f1 (uint x);
  uint f2 (uint x);
  double f3 (double x, double y);
```

### Bedingte Kompilierung

Falls ein Header (absichtlich oder unabsichtlich) mehrfach über `#include` eingebunden wird können Fehler (ambiguity) auftreten.

Eine solche Mehrfacheinbindung kann natürlich offensichtlich passieren...

```c++
#include "a2.h"
#include "a2.h"
```

...oder durch `#include`-Anweisungen in einem *Header*, vor allem wenn dadurch die Vererbung unübersichtlich wird.

In beiden Fällen müssen wir bedingte Kompilierung nutzen:

```c++
#ifndef A2_H
#define A2_H

[...]

#endif
```

*Conditional inclusions* werden auch sehr gut in diesem [Tutorial](https://cplusplus.com/doc/tutorial/preprocessor/#conditional_inclusions) erklärt.

### Flags/Makros bei Kompilierung setzen

Um z.B. Debugging zu betreiben können wir auch in der Konsole Flags setzen, z.B. setzt `g++ -DDEBUG` das Makro "DEBUG"

Nun können wir im Programm selbst folgendes schreiben:

```c++
#ifdef DEBUG
  if (y == 0)
  {
    std::cout << "Fehler: Division durch 0\n";
    exit (0);   // beendet das Programm
  }
#endif
```

## Call by Value vs. Call by Reference

### Call by Value

```c++
int add (int x, int y)
{
  return x + y;
}

int main ()
{
  std::cout << add (3, 5) << "\n";
}
```

### Call by Reference

In C können wir Cally by Reference nur mit Pointern in den Parametern & den Adressen als Argumente realisieren:

```c++
void swap (int *px, int *py)
{
  int t = *px;
  *px = *py;
  *py = t;
}

int main ()
{
  int a = 1, b = 2;
  swap (&a, &b);
}
```

In C++ können wir stattdessen einfach Referenzen in die Parameter schreiben:

```c++
void swap (int &x, int &y)
{
  int t = x;
  x = y;
  y = t;
}

int main ()
{
  int a = 1, b = 2;
  swap (a, b);
}
```

## Defaultwerte

Fehlende Parameter im Funktionsaufruf werden durch einen Defaultwert ersetzt, der in Deklaration oder Definition der Funktion angegeben wird.

Syntax:

```c++
int divide (int a, int b = 2)
{
  int r;
  r = a / b;
  return (r);
}
```

Es gelten zusätzlich folgende Regeln:

- ab dem **ersten Defaultwert** müssen alle folgenden Parameter Defaultwerte besitzen
- beim Aufruf der Funktion darf ab dem **ersten fehlenden Argument** kein weiteres Argument übergeben werden

## Überladen von Funktionen

Funktionen mit gleichem Namen, die sich jedoch folgenderweise unterscheiden müssen:

- eine unterschiedliche Anzahl an Parametern

und / oder

- unterschiedliche Datentypen der Parameter

### Mehrdeutigkeit

Ein Funktionsaufruf ist mehrdeutig, falls der Compiler anhand der Argumente nicht eindeutig eine Funktion auswählen kann.

Gründe:

- es gibt keine Funktion die *exakt* passt, aber mehrere für die die Argumente konvertiert werden können
- es gibt aufgrund von Defaultwerten mehrere Funktionen die exakt mit den Argumenten übereinstimmen.

*Beispiele aus T3/4*

```c++
int max (int x, int y = 0)
  {return x >= y ? x : y;}
long max (int x, int y, long z = 0)
  {return x >= y ? (x >= z ? x : z) : (y >= z ? y : z);}
double max (double x, double y)
  {return x > y ? x : y;}

int main ()
{
  std::cout << max (1) << "\n";              // eindeutig, max(int,int) mit x=1 und y=0
  std::cout << max (1.0) << "\n";            // eindeutig, max(int,int) mit x=1 und y=0
  std::cout << max (1, 2.0, 3L) << "\n";     // eindeutig, max(int,int,long) mit x=1 und y=2 und z=3
  std::cout << max (1.0, 2.0) << "\n";       // eindeutig, max(double,double) mit x=1 und y=2
  std::cout << max (1.0, 2) << "\n";         // mehrdeutig, alle drei Funktionen sind moeglich
  std::cout << max (1, 2) << "\n";           // mehrdeutig, die Parameter passen exakt zu den ersten beiden Funktionen
}

```

# Woche 4

## Variable Parameterliste

Syntax:

```c++
#include "stdarg.h"

int function (int n, ...)
{
  va_list list;
  va_start (list, n);
  
  int sum = 0;
  for (int i = 0; i < n; i++)
    sum += va_arg (list, int);
  
  va_end (list);
  return sum;
}
```

- es braucht immer mind. einen festen Parameter, die variable Parameterliste `...`steht immer am Ende
- der Compiler überprüft beim Aufruf der Funktion immer nur die Argumente für die festen Parameter

`va_start (list, n)` gibt den Anfang der Parameterliste an (n ist dabei der letzte Parameter vor dieser)

`va_arg (list, int)` liefert das aktuelle Listenelement - konvertiert in den geg. Datentyp (hier: `int`) - und setzt den Zeiger auf das nächste Element

`va_end (list)` beendet die Bearbeitung der Parameterliste

### Notizen aus T4/1

- Durch einen geg. string können wir wie gewohnt mit einer `for (char c : s)`-Schleife iterieren
- Um innerhalb eines switch-statements aus einer umgebenden Schleife zu entkommen: `goto end;   end: va_end (list);` nutzen
- Um aus einem geg. ASCII-Wert in `int` einen char zu machen: `(char) va_arg (list, int)`
- Es können in der variablen Parameterliste natürlich auch Pointer übergeben werden: `int *p = va_arg (list, int*)`

## Makros

Kleine Berechnungen in Funktionen sind aufgrund des nötigen Funktionsaufrufs bei kleinen Berechnungen nicht effizient.

Mit Makros führen wir eine Textersetzung für einen Ausdruck durch, die uns den Funktionsaufruf erspart.

Die Makros werden nicht vom Compiler, sondern vom Präprozessor bearbeitet: Keine Typprüfung!

Synatx: `#define Name (Parameter) Ausdruck`

Beispiele:

```c++
#define MAX (A, B) ((A>B) ? A : B))
#define PI  3.141592

#define SWAP(A,B)  \
{                  \
  auto T = A;      \
  A=B;             \
  B=T;             \
}
```

- die Parameter sind optional und erhalten keinen Datenwert
- Makros mit mehreren Zeilen *müssen* hinter jeder Zeile ein `\` stehen haben

Makros können zu vielen Seiteneffekten führen, dazu gehören u.a.:

- mehrfache Ausführung von Inkrement/Dekrement im Makro selbst (z.B. bei MAX(x++, y--))
- bei gleichem Name von Parametern und lokalen Variablen im Makro: Fehler! (z.B. "T" in SWAP)
- Es gibt möglicherweise Probleme bei Anwendung von Makros in Kontrollstrukture, z.B. `if SWAP(a,b)`

## inline Funktionen

Eine Alternative zu Makros: Der Funktionsaufruf wird vermieden, indem der Funktionskörper einfach direkt an der Stelle des Aufrufs eingefügt wird.

Syntax:

```c++
inline double flaeche (double r)
{
  return 3.141592 * r * r;
}
```

Vorteile:

- keine unerwünschten Seiteneffekte
- Typprüfung durch den Compiler
- sonst wie gewöhnliche Funktionen

Nachteile:

- müssen für unterschiedliche Datentypen mehrfah implementiert werden (da Typprüfung)
- das Keyword `inline` ist nur ein Vorschlag für Compiler: Auch Funktionen ohne `inline` können inline werden & umgekehrt => Keine Sicherheit

## Funktionspointer

Jede Funktion besitzt eine Einsprungadresse im Programm, über die sie aufgerufen wird. 

Diese Adresse können wir speichern und aufrufen: `Datentyp (*fp) (Parameter) = f;` => `fp (Parameter)`

```c++
void sort (int x, int y, bool (*fp) (int, int))
{
  if (fp (x, y))
  {
    int t = x;
    x = y;
    y = t;
  }
}

bool compare (int a, int b)
{
  return a >= b;
}

int main ()
{
  sort (7, 2, compare);
}
```

Eine Funktion, die als Funktionspointer in einen Parameter eingesetzt werden kann, nennt man auch "Callback-Funktion".

## Strukturen

[Tutorial](https://www.cplusplus.com/doc/tutorial/structures/)

Syntax:

```c++
struct S
{
  double x;
  int *i;
  struct S **p;
  struct
  {
    char c;
  } a;
};

int main ()
{
  int a [4] = {1,2,3,4};
  S s, *p = &y;
  s.x = 1.2;
  s.i = a;
  s.p = &p;
  s.a.c = 'x';
}
```

Im Beispiel oben ist a eine Strukturvariable, die durch eine *anonyme Struktur* das `char c` als Element erhält.

- In C muss (im Gegensatz zu C++) bei der Deklaration von Strukturvariablen ein `struct` angegeben werden: `struct S s;`
- Eine Strukturvariable kann direkt initialisiert werden: `struct S s = {3.2}`, fehlende Werte werden mit 0 ergänzt.

Bei einem Pointer auf eine Struktur `S *p = &s;` kann über den `->`-Operator auf die Elemente der Struktur zugegriffen werden:

```c++
S *p = &s;
p->x = 12.2;
```

Dieser entspricht der Dereferenzierung mit Punkt, also `p->x == (*p).x`

### Notizen aus T4/4

- `->` wird immer *vor* `&` und `*` ausgewertet => Klammersetzung ist sehr wichtig!
- `.` wird auch immer *vor* `&` und `*` ausgewertet
- Um oben auf den `char c` innerhalb der Anonymen Struktur a zugreifen zu können, müssen wir wie folgt einen Pointer auf a generieren, über den wir auf c zugreifen können: `(&s.a)->c`

## Arrays

[Tutorial](http://cplusplus.com/doc/tutorial/arrays/)

Deklaration & Initialisierung:

```c++
int n = 0;
std::cin >> n; 

int x [];             // illegal
int a [4];            // Feld ohne Initialisierung
int b [4] = {1,2,3};  // Feld mit Initialisierung
int c [] = {1,2,3};   // Feld ohne Groessenangabe
int d [n];            // Feld variabler Laenge (NUR IN C, NICHT IN C++)
```

Wird ein Array nicht initialisiert, so ist der Inhalt unbestimmt:

```c++
int a [4];            // enthält z.B. {3732831,8783274,932493,326473};
int b [4] = {1,2,3}   // enthält {1,2,3,0}  =>  fehlende Werte = 0 (default value)
int c [4] = {}        // enthält {0,0,0,0}  =>  alle Werte = 0 (default value)
```

Die Größe eines Arrays (Anzahl Elemente) lässt sich folgendermaßen bestimmen:

```c++
int a [4] = {1,2,3,4};
int n = sizeof (a) / sizeof (a[0]);   // sizeof (a[0]) == sizeof (int)
```

# Woche 5

## qsort

[cplusplus reference](http://cplusplus.com/reference/cstdlib/qsort/?kw=qsort)

Syntax: `qsort (pointer to array, number of elements, size of each element, compare function)`

```c++
int array [] = {3,7,2,13,5,8,1,17,6,9};
qsort (array, sizeof(array) / sizeof(int), sizeof(int), intCompare);
```
