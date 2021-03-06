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

Die compare-Funktion muss selbst geschrieben werden und folgt folgendem Aufbau:

```c++
int compare (const void* p1, const void* p2)
{
    int i1 = *(const int *) p1;   // konvertiert den void-Pointer in einen int-Pointer und dereferenziert dessen Wert
    int i2 = *(const int *) p2;
    return i1 - i2;
}
```

### Notizen aus T5/1

- Für ein `const char *str [] = {"hallo", "heute"}` Array muss folgendes getan werden:
  - `num = sizeof (str) / sizeof (const char *)`
  - in der compare-Funktion: `const char *s1 = *(const char **) p1;` => Es werden Pointer auf die Elemente des Arrays übergeben, die selbst Pointer auf chars sind (da ein string ja nichts anderes als ein char-Array ist). Wir konvertieren also in einen doppelten char Pointer und dereferenzieren diesen

## C-Strings

[Wichtige Funktionen der Standardbibliothek](http://cplusplus.com/reference/cstring/)

= Feld des Datentyps char, wobei das Ende des Strings mit einem `\0` gekennzeichnet wird

- benötigen `#include <cstring>`
- dürfen nach der Initialiserung nicht mit einem neuen String überschrieben werden (also z.B. `char s1[] = "hello"; s1 = "test"`
  - dafür nutzen wir strcpy
- `strlen (s1)` ergibt die Länge des Strings bis zum `\0` (und ohne dieses mitzuzählen)
- `sizeof (s1)` ergibt dahingegen die Anzahl der Elemente. `char s2[20] = "hello";` hat deshalb auch `sizeof (s2) = 20`

```c++
#include <cstring>

int main ()
{
  char s1[] = "hello";    // entspricht ['h','e','l','l','o','\0'}
}
```

## C++ Strings

Siehe Vorlesung, I can't be fucking bothered

# Woche 6

## Dynamische Speicherallokation

Der Stack ist klein und globale & statische lokale Variablen belegen von Programmstart bis Ende ihren reservierten Speicherbereich.

Zudem wissen wir manchmal (z.B. durch User Input) nicht, wie groß ein Feld zur Laufzeit sein soll.

Lösung: Wir erzeugen große Felder dynamisch zur Laufzeit

### C

`void *malloc (size_t sizeOfMemoryBlock)`

malloc allokiert einen zusammenhängenden Speicherbereich der Größe size

```c++
  int i;
  char *buffer;                     // Pointer für array anlegen
  scanf ("%d", &i);                 // user input für Größe des Speicherbereichs
  
  buffer = (char *) malloc (i+1);   // Speicher allokieren (char = 1 Byte)
  if (buffer == NULL) exit (1);     // Error abfangen
  // [...]
  free (buffer);                    // Speicher freigeben
```

`void *calloc (size_t numOfElements, size_t sizeOfEachElement)`

calloc allokiert einen zusammenhängenden Speicherbereich mit num Elementen der Größe von jeweils size & *initialisiert diese mit 0*

```c++
  int i;
  int *pData;                               // Pointer für array anlegen
  scanf ("%d",&i);                          // user input für Anzahl an Elementen
  
  pData = (int *) calloc (i, sizeof(int));  // Speicher allokieren (i Elemente der Größe int)
  if (pData==NULL) exit (1);                // Error abfangen
  // [...]
  free (pData);                             // Speicher freigeben
```

`void *realloc (void *p, size_t newSize)`

realloc vergrößert/verkleinert den allokierten Speicherbereich an \*p auf die Größe size

- Falls der Speicherbereich verkleinert oder verschoben wird, wird der nun nicht mehr genutzte Speicherbereich freigegeben
  - Wir *müssen* dann mit dem neuen (zurückgegebenen) Pointer weiterarbeiten!

`void free (void *p)`

Gibt den allokierten Speicherbereich ab \*p frei

### C++

`new` allokiert einen zusammenhängenden Speicherbereich und gibt einen entsprechenden Pointer zurück

`delete` gibt den zusammenhängenden Speicherbereich frei

- Dynamische Speicherbefehle von C & C++ dürfen *nicht* vermischt werden!
- Die C++ Operationen rufen jeweils den entsprechenden Konstruktor bzw. Destruktor auf

Beispiel:

```c++
struct Time {
  int hour, min, sec;
}

int main () {
  int *a1 = new int[5];             // keine Initialisierung
  int *a2 = new int[5] {1,2,3};     // Initialisierung mit {1,2,3,0,0}
  int *a3 = new int[5] ();          // Initialisierung mit {0,0,0,0,0}
  int *t1 = new Time;               // keine Initialisierung
  int *t2 = new Time {12,30}        // Initialisierung mit {12,30,0}
  int *t3 = new Time ();            // Initialisierung mit {0,0,0}
  
  delete [] a1; delete [] a2; delete [] a3;   // alle mit "new type[]" erzeugten Pointer müssen mit "delete []" freigegeben werden
  delete t1; delete t2; delete t3;            // alle mit "new type" erzeugten Pointer müssen mit "delete" freigegeben werden
}
```

### Probleme der Speicherverwaltung

- Memory Leaks: Ein Speicherbereich kann nicht mehr verwaltet werden, weil der Pointer überschrieben wurde
- Dangling Pointer: Ein Speicherbereich wird über 2 versch. Pointer referenziert und über den ersten Pointer freigegeben. Die Adresse im zweiten Pointer wird dann ungültig (`double free error` sollten wir dann versuchen den Speicherbereich über den zweiten Pointer freizugeben)

### Felder zurückgeben

Adressen für arrays sind nur lokal, weswegen wir Felder auf dem Heap allokieren müssen, um sie nach der Methode weiterverwenden zu können.

```c++
int *f ()
{
  int *a = new int[4] {1,2,3,4};    // Feld wird auf Heap allokiert
  return a;                         // wir geben in Form eines Pointers die Adresse des Felds zurück
}

int main ()
{
  int *a = f ();                    // wir übernehmen den Pointer...
  a[0] = 5;                         // können ihn nutzen um das Feld zu lesen / zu bearbeiten
  delete a;                         // und MÜSSEN ihn am Ende auch freigeben!
}
```

### Zweidimensionale Felder

- Methode 1: Ein Zusammenhängender Bereich
  - Einfache Allokation, kompliziertere Indexberechnung
- Methode 2: Zweidimensionale Felder mit Pointern
  - Kompliziertere Allokation, einfache Indexberechnung

Zu Methode 2:

```c++
// Allokieren
int (*feld) [3] = new int [2][3] {{1,2,3},{2,3,5}}; 
// Freigeben
delete [] feld;

// ODER

// Allokieren
int **feld = new int* [2];
for (int i = 0; i < 2; i++)
  feld[i] = new int [3];
// Freigeben
for (int i = 0; i < 2; i++)
  delete [] feld[i];
delete feld;
```

### Ragged Arrays

siehe VL

## Listen

siehe VL

# Woche 7

## Klassen

*Sehr* gute Zusammenfassung in VL & auf cplusplus.com

Auszüge:

### Zugriffsrechte

- `private` Zugriff *nur in der Klasse* zulässig
- `protected` Zugriff *nur in der Klasse* UND *in allen abgeleiteten Klassen* zulässig
- `public` Zugriff aus *allen Klassen* UND *globalen Funktionen* zulässig

Einziger Unterschied zwischen `struct` und `class` ist das Defaultzugriffsrecht:

- `struct` = `public`
- `class` = `private`

#### friend-Deklaration

Andere Klassen und Funktionen können in einer Klasse als `friend` deklariert werden

Diese erhalten damit Zugriff auf alle `private` und `protected` Attribute & Methoden der Klasse

```c++
class Konto
{
  friend void ausgabe (const Konto &konto); // "ausgabe" kann damit auf alle Attribute & Methoden von "Konto" zugreifen
}

void ausgabe (const Konto &konto)
{ 
[...] 
}
```

### const-Methoden

Durch eine `const`-Deklaration teilen wir dem Compiler mit, dass in der markierten Methode keine *Instanzvariable* der Klasse geändert wird.

- Vergleiche mit `const`-Parameter: Damit teilen wir dem Compiler mit, dass die übergebenen Argumente (falls Referenzen/Pointer) nicht verändert werden!
- `const`-Methoden selbst dürfen auch nur andere `const`-Methoden aufrufen (selbst wenn diese eigentlich keine Instanzvariablen ändern!)
- Wir dürfen für `const`-Instanzen einer Klasse *nicht* non-`const`-Methoden aufrufen!

```c++
void ausgabe () const
{

}
```

=> Ausführliches Beispiel in der VL

### Konstruktor

Wird beim Erzeugen einer Instanz der Klasse aufgerufen

- Trägt den Namen der Klasse und hat *keinen* Rückgabetyp
- Eine Klasse kann mehrere unterschiedliche Konstruktoren besitzen => gleiche Regeln wie beim Überladen von Funktionen
- Wird kein Konstruktor definiert, so ergänzt der Compiler den parameterlosen Default-Konstruktor

```c++
class Konto
{
  public:
    Konto () {}
    Konto (int i) {}
}
```

### Destruktor

Wird beim Freigeben einer Instanz der Klasse aufgerufen

- Eine Klasse darf *nur einen* Destruktor ohne Parameter & Rückgabetyp besitzen
- Wird kein Destruktor definiert, so ergänzt der Compiler einen *leere* Destruktor

```c++
class Konto 
{
  public:
    ~Konto ()
    {
      [...]
    }
}
```

### Initialisierungsliste

[Tutorial zur Initialisierungsliste](https://cplusplus.com/doc/tutorial/classes/#member_initialization)

```c++
class Konto
{
  private:
    int summe;
  public:
    Konto (int s) : summe(s) 
    {
    
    }
}
```

Vorteile der Initialisierungsliste:

- mehrfache Initialisierung von Instanzvariablen (also Objekten anderer Klassen) wird verhindert
  - Beispiel:

```c++
class A {
public:
    int x;
    A() { x = 0; }
    A(int i) { x = i; }
};

class B {
public:
    B() { a.x = 3; }    // doppelte Initialisierung von a.x => erst x = 0 durch Default-Konstrukto von a und danach x = 3
    // vs.
    B() : a(3) { };     // einfache Initialisierung von a.x => direkt der Aufruf des A(3) Konstruktors, damit also nur x = 3
private:
    A a;
};
```

- `const`-Variablen und Referenzen können *nur* in der Initialisierungsliste, *nicht* im Konstruktorkörper initialisiert werden
- für Instanzen einer Klasse könenn wir explizite Konstruktoraufrufe durchführen (siehe Beispiel oben zu mehrfacher Initialisierung)
- wir können einen anderen Konstruktor der gleichen Klasse aufrufen

Eine weitere wichtige Funktion der Initialisierungsliste ist die Allokierung von Speicher (per `new`):

```c++
class Klasse
{
  private:
    string *schueler;
  public:
    Klasse (int num, string *namen) : schueler(new string[num])
    {
      for (int i = 0; i < num; i++)
        schueler[i] = namen[i];
    }
}
```

### Tutorial zu [Initialisierung von Objekten](https://cplusplus.com/doc/tutorial/classes/#uniform_initialization)

### Operatoren-Überblick


| expression | can be read as                                                       |
| ---------- | -------------------------------------------------------------------- |
| \*x	       | pointed to by x                                                      |
| &x	       | address of x                                                         |
| x.y	       | member y of object x                                                 |
| x->y	     | member y of object pointed to by x                                   |
| ( \*x).y	 | member y of object pointed to by x (equivalent to the previous one)  |
| x[0]	     | first object pointed to by x                                         |
| x[1]	     | second object pointed to by x                                        |
| x[n]	     | (n+1)th object pointed to by x                                       |

# Woche 8

## Copy-Konstruktor

Der Copy-Konstruktor sowie der Copy-Zuweisungsoperator werden automatisch vom Compiler ergänzt, falls keine eigenen definiert werden.

Ein eigener Copy-Konstruktor wird z.B. in folgendem Fall nötig:

```c++
String s1 = ("Hello");
// Alle folgenden Operationen machen für unsere eigene Klasse `String` das gleiche:
String s2 (s1);
String s2 {s1};
String s2 = s1;
```

Problem: Änderungen an `s2` verändern *auch* `s1`! Daher schreiben wir unseren eigenen Copy-Konstruktor:

```c++
class String {
  private:
    string str;
  public:
    // Copy-Konstruktor
    String (const String &s) 
    : str (new char [strlen (s.str) + 1]) // Arrays immer in Initialisierungsliste allokieren...
    {
      strcpy (str, s.str);                // ...und dann im Funktionskörper füllen
    }
    
    // Copy-Zuweisungsoperator
    const String &operator= (const String &s) {
      if (&s != this)                         // !!! verhindert Probleme bei Selbstzuweisung, also s1 = s1
      {
        delete str;                           // Puffer freigeben
        str = new char [strlen(s.str) + 1];   // neuen Speicher mit entsprechender Größe allokieren
        strcpy (str, s.str);                  // diesen füllen
        return *this;                         // !!! Das eigene Objekt dereferenziert zurückgeben
      } 
    }
}
```

- Die Rückgabe `return *this` ist wichtig für Mehrfachzuweisungen, also z.B. `s1 = s2 = s3;`
- Der Copy-Konstruktor lässt sich im Copy-Zuweisungsoperator nutzen & umgekehrt => spart 2x das gleiche zu machen

ACHTUNG:

```c++
int main ()
{
  A a1;       // Default-Konstruktor
  A a2 (a1);  // Copy-Konstruktor
  A a3 = a1;  // Copy-Konstruktor - NICHT Zuweisungsoperator: '=' ist hier der Initialisierungsoperator!!!
  a3 = a2;    // Zuweisungsoperator
}
```

Problem: Das kopieren der Daten per Copy-Konstruktor kann schnell langsam werden, deshalb alternativ: 

## Move-Konstruktor

### L-Value vs. R-Value

`L-Value Objekt`: Ausdruck, dessen Adresse vom Programm bestimmt werden kann

`R-Value Objekt`: Ausdruck, dessen Adresse *nicht* vom Programm bestimmt werden kann

`L-Value Referenz (&)`: Referenz auf ein L-Value Objekt

`R-Value Referenz (&&)`: Referenz auf ein R-Value Objekt

Beispiel:

```c++
int i = 1;

int &l1 = i;      // zulässig, i ist L-Value
int &l2 = i+2;    // nicht zulässig, i+2 ist R-Value
int &&r1 = i+2;   // R-Value-Referenz
int &&r2 = i;     // implizit nicht zulässig, da i ist L-Value
```

### Move-Konstruktor und Move-Zuweisungsoperator

- Die *Move-Methoden* werden aufgerufen, wenn der Quell-Operand eine `R-Value Referenz` ist
  - Ist der Quell-Operand eine `L-Value Referenz`, so werden die *Copy-Methoden* aufgerufen
- Eine `L-Value Referenz` kann mit `std::move(l_value_reference)` in eine `R-Value Referenz` umgewandelt werden
- Der Aufruf von `std::move()` auf eine `const L-Value Referenz` hat keine Wirkung

Beispiele dazu ob Copy- oder Move-Methoden aufgerufen werden: VL 07/153

```c++
    int *array, size;
	// Move-Konstruktor
    A (A &&a) : array (nullptr)
    {
      *this = std::move (a);	// wir nutzen den Zuweisungsoperator
    }
	
	// Move-Zuweisungsoperator
    const A &operator= (A &&a)
    {
      delete [] array;
      array = a.array;
      size = a.size;
      a.array = nullptr;
      a.size = 0;
      return *this;
    }
```

## Klassenvariablen und Klassenmethoden (static)

[Tutorial](https://cplusplus.com/doc/tutorial/templates/#static_members)

### Klassenvariablen

- nur einmal für die gesamte Klasse erzeugt
- nicht an eine Instanz der Klasse gebunden, der Wert der Klassenvariable ist für alle Instanzen gleich
- mit dem Keyword `static` gekennzeichnet: `static int numObjects;`
- Initialisierung erfolgt *außerhalb* der Klassendefinition: `int Klasse::numObjects = 0;`

Auf die Variable zugreifen können wir...

- ...über jede Instanz der Klasse: `Klasse a;` `a.numObjects = 0;`
- ...über den Scope-Operator: `Klasse::numObjects = 0;`

### Klassenmethoden

- benötigt keine Instanz der Klasse um aufgerufen zu werden
- innerhalb dieser darf kein `this` benutzt werden
- sie haben außerdem *nur* Zugriff auf `static` Klassenvariablen und dürfen *nur* `static` Klassenmethoden aufrufen
  - umgekehrt dürfen sie aber von allen Instanzmethoden genutzt werden

Die Klassenmethode aufrufen können wir...

- ...über jede Instanz der Klasse: `Klasse a;` `a.getNumObjects();`
- ...über den Scope-Operator: `Klasse::getNumObjects();`

## Überladen von Operatoren

[Tutorial](https://cplusplus.com/doc/tutorial/templates/#overloading_operators)

Durch das Überladen von Operatoren lassen sich diese auf Instanzen eigener Datentypen (Klassen) anwenden.

Sie können sowohl als Instanzmethode oder als globale Funktion überladen werden.

Wird ein Parameter gefordert, so ist dieser immer eine `&` Referenz und - wenn der Parameter nur gelesen wird - `const`.

### Instanzmethode

Der erste Operand ist die Instanz auf die die Instanzmethode aufgeführt wird, der evtl. zweite Operand ist im Parameter.

### globale Funktion

Der einzige / beide Operanden sind im Parameter.

Beispiel für beide Optionen:

```c++
class Klasse
{
  public:
    int a;
    Klasse (int a) : a(a) {}
  // Instanzmethode:
    Klasse operator+ (const Klasse &k) {return a + k.a;}
}

// ODER globale Funktion
Klasse operator+ (const Klasse &k1, const Klasse &k2)
{
  return k1.a + k2.a;
}

int main ()
{
  Klasse k1 = 2, k2 = 4;
  Klasse k3 = k1 + k2;    // k3.a = 6
}
```

Wir können eine globale Operator-Funktion natürlich innerhalb einer Klasse als `friend` deklarieren. Darüber hinaus können wir sie auch wie folgt innerhalb der Klasse definieren, wobei dies keinen Unterschied zu der Definition auserhalb der Klasse hat:

```c++
class A
{
  int value;
  
  bool operator< (const A a2)
  {
    return value < a2.value;
  }
// vs.
  friend bool operator< (const A &a1, const A &a2)
  {
    return a1.value < a2.value;
  }
}
```

Es folgen einige häufig überladene Operatoren und ihre Eigenheiten:

### +=,-=,\*=,/= Zuweisungsoperator

Diese berufen sich im besten Fall auf ihre eigentlichen Operator-Methoden (also z.B. operator+ für operator+=) und geben die Instanz selbst wieder aus:

```c++
const A &operator+= (const A &a)
{
  *this = *this + a;  
  return *this;
}
```

### Vergleichsoperatoren

```c++
bool operator< (const A &a1, const A &a2)
{
  return a1.value < a2.value;
}
```

### Inkrement- / Dekrement-Operatoren (Prä/Post)

Der Post-Operator `i++` wird durch einen Dummy-Parameter des Typs `int` markiert

```c++
class A
{
  int x;
  // Prä-Operator   (++a)
  A &operator++ () 
  {
    x++;
    return *this;
  }
  // Post-Operator  (a++)
  A &operator++ (int)
  {
    A r (x);    
    x++;
    return r;   // wir geben also eine Instanz von A zurück, die noch den "alten" Wert von x hält
  }
}

// als globale Funktion muss man die Parameter wie folgt schreiben:
// ++a
A operator++ (Rational &a) {}
// a++
A operator++ (Rational &a, int) {}
```

### Typecast

- darf *nur* als Instanzmethode überladen werden
- es können Mehrdeutigkeiten entstehen, wenn mehrere Operatoren oder ein Konstruktor und ein Typecast die Umwandlung durchführen *könnten*
  - Beispiele dazu in der VL 07/161

Form: `operator TYPE () const {}`  mit TYPE als Datentyp in den umgewandelt wird

Beispiel:

```c++
class A
{
  int i;
  operator int () const   // Umwandlung in ein int
  {
    return i;
  }
  operator B () const     // Umwandlung in ein B
  {
    return B (i);         // wir nutzen den Konstruktor von B mit Parameter i
  }
}
``` 

### Ein- und Ausgabe-Operatoren (>>,<<)

```c++
// Eingabe: 
// Parameter = Referenz auf instream sowie auf unser Objekt
std::istream &operator>> (std::istream &is, Konto &konto) 
{
  is >> konto.inhaber >> konto.kontonummer;     // den instream modifizieren
  return is;                                    // und dann ausgeben
}

// Ausgabe:
// Parameter = Referenz auf ostream sowie auf unser CONST Objekt
std::ostream &operator<< (std::ostream &os, const Konto &konto) // WICHTIG: const im Objekt-Parameter, da wir dieses nicht verändern
{
  os << "Inhaber: " << konto.inhaber << "\n";
  return os
}
```

# Woche 9

## Callable Objects

Callable Objects können als Parameter an Funktionen übergeben werden, um innerhalb dieser versch. Funktionalitäten zu erfüllen (z.B. kann man unterschiedliche `compare`-Funktionen an `qsort` übergeben).

In C++ sind dies Funktionspointer, Funktoren und Lambda-Ausdrücke.

### Funktionspointer

Wurden zuvor schon vorgestellt: `type (*f) (parameters)` als Parameter an die Funktion und dann den Funktionsnamen (z.B. `compare`) als Argument übergeben. 

Beispiel:

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

### Funktoren

= Klassen, in denen der Funktions-Operator `type operator() (parameters)` überladen wird.

Der Funktions-Operator kann auch mehrfach mit jeweils unterschiedlichen Parametern überladen werden.

Eine Instanz dieser Klasse kann dann wie wie ein Funktionsname zum Aufruf dieser Operatoren genutzt werden:

```c++
class Add                             // Funktor
{
  public:
    int operator () (int a, int b)    // operator() wird überladen
    {
      return a + b;
    }
};

int main ()
{
  Add add;                            // Instanz des Funktors
  int x = add (3,5);                  // wir rufen den operator() auf die Instanz des Funktors auf
}
```

Funktoren sind flexibler als Funktionspointer, da sie in den jeweiligen Instanzen Instanzvariablen speichern können (d.h. wir können diese auch in späteren Aufrufen der Funktor-Instanz noch verwenden).

#### Ändern von externen Variablen per Funktor

Um lokale Variablen in anderen Funktionen bzw. der main-Funktion innerhalb des Funktors verändern zu können, müssen wir in dessen Konstruktor eine Referenz auf diese lokale Variable übergeben:

```c++
class Mult
{
  private:
    int &y;
  public:
    Mult (int &y) : y (y) {}

    void operator () (int a)
    {
      y *= a;
    }
};

int main ()
{
  int y = 3;
  Mult mult (y);
  mult (5);       // y *= 5 => y = 3 * 5 = 15
}
```

#### Funktoren als Parameter

Funktoren lassen sich wie Funktionspointer als Argument an andere Funktionen übergeben:

```c++
class Functor
{
  private:
    int k;
  public:
    Functor (int k) : k(k) {}               // Konstruktor
    bool operator() (int a) {return k==a;}  // überladener operator()
}

int sum (int n, Functor f)                  // Functor im Parameter
{
  return f(n) ? 1 : 0;                      // operator() nutzen
}

int main ()
{
  // Wir rufen sum() mit einer per Konstruktor neu erzeugten Instanz von Functor auf
  cout << "Dies hat keinen wirklichen Sinn: " << sum (10, Functor (3)) << "\n";
}
```

### Lambda-Ausdrücke

= anonyme Funktion, die typischerweise direkt am Ort des Aufrufs bzw. bei der Parameterübergabe geschrieben wird.

Alternativ können die Lambda-Ausdrücke auch für eine häufigere Verwendung als Variablen in der Methode/Klasse deklariert & definiert werden.

Syntax: `[capture] (parameter) -> return_type {function_body}`

- `[capture]`: legt fest, wie schon bestehende Variablen im Erstellungskontext an den Lambda-Ausdruck gebunden werden können:
  - `[]` = Es wird keine Variable gebunden (Default, die "\[]" **müssen** aber geschrieben werden!
  - `[=]` = Kopie als Standard-Zugriff auf *alle* Variablen
  - `[&]` = Referenzen als Standard-Zugriff auf *alle* Variablen => ermöglicht Änderungen (duhhh)
  - `[x]` = Kopie als Zugriff auf die Variable x
  - `[&x]` = Referenz als Zugriff auf die Variable x => **WICHTIG**, falls sich x zw. Aufrufen des Lambda-Ausdrucks ändert

Es können auch mehrere Variablen *gecaptured* werden, diese müssen dann per Komma getrennt werden: `[x, &y]`

Wollen wir *alle* Variablen *bis auf z* als Referenz und z als Kopie capturen, so schreiben wir `[&, z]` (gleiches für alle anderen Fälle).

- `(parameter)`: übergibt wie bei Funktionen Argumente. Alternativ zur `[capture]`-Klausel können hier auch Kopien oder Referenzen übergeben werden.
- `-> return_type`: kann weggelassen werden, wenn der Compiler den return_type selbst ableiten kann

Beispiel:

```c++
int main ()
{
  int x = 3, y = 5;

  // 1. Variablen zur späteren Verwendung als Funktion

  // Referenz auf Variablen über Parameter
  auto swap  = [] (int &a, int &b) -> void {int t = a; a = b; b = t;};
  swap (x, y);

  // Referenz auf Variablen über capture-Klausel
  auto print = [&] () -> void {std::cout << "x=" << x << "," << "y=" << y << "\n";};
  print ();
}

// 2. Deklaration & Definition direkt am Ausführungsort, z.B. bei for_each

template<typename T>
void print (const std::vector<T> &container)
{
  for_each (container.begin (), container.end (), 
              [] (T x) -> void {std::cout << x << " ";} ); 
}

```

#### Lambda-Ausdrücke & Funktionspointer

Lambda-Ausdrücke *ohne* `[capture]`-Klausel dürfen in einen Funktionspointer mit **gleichen** Parametern & **gleichen** Rückgabetyp gespeichert werden

Umgekehrt (Funktionspointer in Lambda-Ausdruck speichern) ist dies *nicht* möglich.

Beispiel:

```c++
bool isEven (int i)
{
  return i % 2 == 0;
}

int main ()
{
  auto lambdaEven = [] (int i) {return i % 2 == 0; }  // macht das gleiche wie die Funktion isEven()
  bool (*fpEven) (int) = isEven;                      // Funktionspointer auf isEven
  
  fpEven = lambdaEven;                                // zulässig
  lambdaEven = fpEven;                                // nicht zulässig
}
```

## Vererbung

= eine Klasse wird von einer Oberklasse abgeleitet und übernimmt dadurch automatisch alle Attribute & Methoden der Oberklasse

```c++
class Subclass : public Superclass
{
}
```

Mit dem Spezifizierer `public` und seinen Alternativen können die Zugriffsrechte auf die geerbten Attribute und Methoden eingeschränkt werden:

- `public`: gleiche Zugriffsrechte wie in Oberklasse
- `protected`: public -> protected, private bleibt gleich
- `private`: public & protected -> private

### Konstruktor- & Destruktor-Aufruf

Es ist immer schlauer Attribute der Oberklasse, die von dessen Konstruktor gesetzt werden, auch über diesen zu setzen, da sonst (unnötigerweise) das Attribut erst über dessen Default-Konstrukto gesetzt wird.

```c++
class Person
{
  protected: 
    string name;
  public:
    Person () {}
    Person (string name) : name(name) {}
}

class Mitarbeiter
{
  private:
    double gehalt;
  public:
    // Konstruktoraufruf von Person mit name als Argument
    Mitarbeiter (string name, int gehalt) : Person (name), gehalt (gehalt) {}
}
```

Fehlt ein solcher expliziter Aufruf des Konstruktors der Oberklasse in der Initialisierungsliste, so wird automatisch der Default-Konstruktor der Oberklasse aufgerufen.

#### Reihenfolge

- **Konstruktor:** Oberklasse -> Unterklasse
- **Destruktor:** Unterklasse -> Oberklasse

### Überschreiben von Methoden

Methoden der Oberklasse werden durch *gleichnamige* Methoden der Unterklasse **verdeckt**.

- verdeckte Methoden lassen sich explizit über den Bereichsoperator `::` ausführen: `Person::get()`
- mit `using` lassen sich...
  - verdeckte Methoden sichtbar machen

```c++
class Mitarbeiter : public Person
{
  using Person::get;    // Person::get wird eigentlich von double get (double d) verdeckt, hiermit aber sichtbar gemacht
  double get (double wechselkurs) {return wechselkurs * gehalt;}
  // ACHTUNG: Sollten beide Parametergleich sein, so erhalten wir natürlich einen ambiguity error!
}
```

  - Zugriffsrechte in der Unterklasse ändern

```c++
class Mitarbeiter : public Person
{
  private:
    Person::name;   // name war in Person (und damit auch in Mitarbeiter) eigentlich protected,
                    // jetzt ist es _in Mitarbeiter_ private
}
```

### Typumwandlung zwischen Ober- und Unterklasse

#### Typumwandlung für Instanzen

- Unterklasse -> Oberklasse (`Unterklasse u; Oberklasse o = u;`)
  - zulässig, Copy-Konstrukto der Oberklasse wird ausgeführt
- Oberklasse -> Unterklasse (`Oberklasse o; Unterklasse u = o;`)
  - **nur** zulässig, falls die Unterklasse einen Konstruktor mit der Oberklasse als Parameter besitzt **ODER** der Typecast-Operator der Oberklasse überladen wurde

#### Typumwandlung für Pointer & Referenzen

Mut zur Lücke oder so

Beispiel:

```c++
class B;                                  // Typecast: Forward Deklaration notwendig

class A {
  private:
    int x;
  public:
    A (int x) : x (x) {}
    int getX () const {return x;}
    operator B ();                        // Typecast: Nur Deklaration möglich
};

class B : public A {
  private:
    int y;
  public:
    B (int x, int y) : A (x), y (y) {}
    B (const A &a) : A (a), y (0) {}      // Konstruktor zur Umwandlung
    int getY () const {return y;}
};

A::operator B () {                        // Typecast: Definition zur Umwandlung
    return B (x, 0);
}

int main ()
{
  A a1 (1);
  B b1 (2, 3);
  A a2 (b1);  // implizite Umwandlung
  B b2 (a1);  // Umwandlung mit Konstruktor
  b2 = a1;    // Umwandlung mit Typecast
}
```

## Virtuelle Methoden

Zusammenfassung siehe VL

### Short summary:

`virtual` entspricht in etwa "abstract" aus Java, aber ich erkläre das nochmal genauer:

Wenn wir mit einem Pointer/einer Referenz einer Oberklasse auf eine Instanz einer Unterklasse arbeiten, 

- also `Basisklasse *r1 = Unterklasse`
- oder `Basisklasse &r2 = Unterklasse`

und auf diesen Pointer/Referenz dann eine Methode aufrufen, die namensgleich sowohl in Oberklasse als auch in Unterklasse existiert, so entscheidet sich am keyword `virtual` in der Methode der Oberklasse, ob diese selbst oder eben die Methode aus der Unterklasse aufgerufen wird!

=> Wenn die Methode der Oberklasse `virtual` deklariert wurde, so arbeiten wir mit der Methode der Unterklasse

=> Andernfalls nennt man die Methode der Oberklasse "statisch" und *diese* wird ausgeführt!

# Woche 10

## Abstrakte Klassen

"rein virtuelle Methode" = virtuelle Methode *ohne* Implementierung `virtual return_type f () = 0`

- Klassen, die mind. eine rein virtuelle Methode besitzen (es müssen nicht *alle* Methoden rein virtuell sein) heißen **abstrakt** und *dürfen nicht* instanziiert werden.
- Eine abstrakte Klasse kann an eine andere Klasse vererbt werden. Die Unterklasse ist *ebenfalls* **abstrakt**, falls nicht *alle* rein virtuellen Methoden implementiert werden.

Beispiel:

```c++
class A {
  public: 
    virtual void f () = 0;    // rein virtuelle Methode
};

class B : public A {
  public: 
    void f () {}              // B ist nicht abstrakt
    // f() könnte auch virtual sein, hauptsache sie ist implementiert
};

int main () {
  A a;    // nicht zulaessig, A ist abstrakt
  B b;    // zulaessig, Unterklasse ist nicht abstrakt
}
```

## Mehrfachvererbung

Eine Klasse kann mehrere direkte Oberklassen haben (im Gegensatz zu Java).

Syntax:

```c++
class Subclass : public Superclass_1, ..., protected Superclass_N
{
}
```

Beispiele dazu siehe VL 08

### Probleme:

- Mehrdeutigkeit durch gleichnamige Objekte in verschiedenen Oberklassen
  - dies lässt sich auch *nicht* durch `::` oder eine Typumwandlung lösen 
- Eine Klasse kann eine Oberklasse *mehrfach* erben
  - problematisch, da - falls die Klasse C z.B. A & B erbt, aber B auch von A erbt - C eine Variable von A nutzen will, diese aber von der Variable in B (die auch durch A ererbt wurde) verdeckt wird

### Lösung über virtuelle Basisklassen

Wir können Basisklassen selbst auch über `class Subclass : virtual public Superclass` virtuell vererben, um Mehrfachvererbung zu vermeiden (beim Beispiel oben: A virtuell an B vererben => C erbt A nur einmal).

## C++ Typecasts

[Ausführliches Tutorial auf cplusplus.com](https://cplusplus.com/doc/tutorial/typecasting/#type_casting)

Der C-Typecast-Operator `operator type ()` ist sehr weitreichend, aber häufig problematisch (runtime errors).

Es gibt deshalb in C++ vier weitere Cast-Operatoren, die eine bessere Kontrolle über die Typumwandlung ermöglichen.

## Vererbung bei Funktoren

Funktoren können wie gewöhnliche Klassen vererbt und ihre Funktionsoperatoren `return_type operator() (parameters)` als (rein-) virtuelle Methoden definiert werden. 

Diese können dann in abgeleiteten Klassen verschieden implementiert werden.

# Woche 11

## C Streams

[Reference auf cplusplus.com](https://cplusplus.com/reference/cstdio/)

In der Standardbibliothek gibt es zwei Ebenen an Funktionen für den Zugriff auf Dateien & Streams:

- Low-Level-Ebene: 	Zugriff erfolgt direkt & synchron
- **High-Level-Ebene**: Zugriff erfolgt asynchron über einen Puffer

### Datei öffnen & schließen

`FILE *fopen (const char *path_name, const char *modus)`

Öffnet die Datei am gegebenen Pfad im genannten Modus und liefert einen FILE \*Pointer.

Der Modus legt die Zugriffsart fest, hier eine Auswahl:

- "r" = Lesezugriff
- "w" = Schreibzugriff
- "b" = Binärdatei, "wb" schreibt also in eine Binärdatei

Im Falle eines Fehlers (z.B. invald path) wird anstatt eines FILE \*Pointers ein `nullptr` zurückgegeben - deshalb **müssen** wir einen Fehlerabfang einbauen!

`int fclose (FILE *file)`

Schließt den Stream des FILE \*Pointers. Gibt `0` im Erfolgsfall und `EOF` bei einem Fehler zurück.

Beispiel:

```c++
  const char *filename = "...";
  FILE *fp = fopen (filename, "w");   // Schreibzugriff

  if (fp != nullptr)    // Fehlerabfang
  {
    // [...]            // ARBEITEN MIT DEM STREAM
    fclose (fp);
  }
```

### Stream schreiben / lesen

Alle folgenden Funktionen sind auf cplusplus.com genauer (und mit Beispielen) erläutert)

Formatierte Ein- und Ausgabe:

- `int fprintf (FILE *file, const char *format, ...);`
- `int fscanf (FILE *file, const char *format, ...);`

  - Der Aufbau des Formatstrings entspricht den Funktionen printf und scanf.

Ein- und Ausgabe von ASCII-Zeichen:

- `int fgetc (FILE *file);`
- `int fputc (int c, FILE *file);`

Ein- und Ausgabe von Strings:

- `int fgets (char *buffer, int size, FILE *file);`
- `int fputs (const char *buffer, FILE *file);`

  - fgets liest die Zeichen vom Stream bis zum ersten Zeilenendezeichen, jedoch max. size-1 Zeichen und speichert diese zusammen mit dem Zeilenendezeichen im char-Buffer.

Blockweise lesen und schreiben:

- `size_t fread (void *p, size_t size, size_t num, FILE *fp);`
- `size_t fwrite (const void *p, size_t size, size_t num, FILE *fp);`
  - Der Ruckgabewert ist die Anzahl der Bl ¨ ¨ocke die gelesen bzw. geschrieben wurden.

## C++ Streams

Vergleich zu C Streams:

- strengere Typ-Prüfung
- flexibel und erweiterbar
- unterstützen Exceptions

### Formatierung von istream & ostream

Die Ein- & Ausgabe über die Shift-Operatoren << und >> kann mit Hilfe von [flags](https://www.cplusplus.com/reference/ios/) formatiert werden.

Beispiel:

```c++
int n = -77;
cout.width(6);              // Breite MUSS für left/right gesetzt sein!
cout << right << n << '\n';  
```

Mehrere Flags "sammeln" sich bis zum nächsten Element das ausgegeben werden soll.

`width` wird z.B. direkt nach der Anwendung auf ein Element zurückgesetzt.

### Manipulatoren 

Die Formatierung der Ein- und Ausgabe kann auch mit Hilfe von Manipulatoren erfolgen.

Manipulator = Funktionspointer / Instanz einer Klasse, der/die direkt mit den Ein-/Ausgabeoperatoren in den Stream geschrieben werden.

*MISSING*: **Eigene Manipulatoren**

### File-Streams

[Tutorial auf cplusplus.com](https://cplusplus.com/doc/tutorial/files/)

# Woche 12

## Templates

Generische Programmierung = Programmierung mit Datentypen als Parameter

=> ermöglicht die Wiederverwendbarkeit von Programmcode ohne Vererbungsmechanismen.

### Funktionstemplates

[Tutorial](https://www.cplusplus.com/doc/tutorial/functions2/#templates)

= globale Funktion für generische Datentypen

- Templateparameter = Variablen / ganzzahlige Werte, die sich als `constexpr` ableiten lassen könnten.
- Default-Werte sind für diese zulässig

Beispiel: 

```c++
template <typename T, int ADD = 0>   // ADD ist ein Templateparameter mit Default-Wert 0
T sum (T a, T b)
{
  T result;
  result = a + b + ADD;
  return result;
}

int main ()
{
  int c = sum<int,3> (2,5);   // = 2 + 5 + 3 = 10
}
```

Werden mehrere `typename` angegeben, so bezieht sich der 1. `typename` auf den 1. Parameter, der 2. auf den 2. usw. 

#### Automatische Typableitung

Für fehlende Templateparameter im Aufruf eines Funktionstemplates führt der Compiler ein automatische Typableitung durch.

- falls die Ableitung nicht klappt (z.B. weil der Datentyp nicht eindeutig ist) wirft der Compiler einen Error
- der Rückgabewert spielt bei der Auswahl keine Rolle
  - dafür achtet der Compiler darauf, dass möglichst wenige `typename` benötigt werden

#### Spezialisierungen

In einer Spezialisierung eines Funktionstemplates werden ein oder mehrere Typvariablen durch konkrete Datentypen ersetzt.

Beispiel:

```c++
template <typename T1, typename T2> T1 add  (T1 v1, T2 v2)     {return v1 + v2;}
// Spezialisierungen von der 1. add Funktion
template <typename T1>              T1 add  (T1 v1, int v2)    {return v1 + v2;}    // T2 durch int ersetzt
template <typename T2>              T2 add  (double v1, T2 v2) {return v1 + v2;}    // T1 durch double ersetzt
// Spezialisierung von der 2. / 3. add Funktion
template <>                         int add (int v1, int v2)   {return v1 + v2;}    // T2 in der Rückgabe durch int ersetzt
```

Der Compiler wählt aus allen zulässigen Spezialisierungen die spezifischste Variante.

Dabei kann es auch zu Mehrdeutigkeiten mit `runtime error` kommen!

### Klassentemplates

[Tutorial](https://www.cplusplus.com/doc/tutorial/templates/#class_templates)

Template-Parameter: gleiche Regeln wie für Funktionstemplates.

Seit C++17 ist eine automatische Typableitung anhand der Parameter des Konstruktors möglich.

Beispiel: 

```c++
template<typename T, int SIZE = 10> class array {
  private: 
    T a [SIZE];
  public:
    array (T init) {    // Konstruktor
      for (int i = 0; i < SIZE; i++) a[i] = init;
    }
    T &operator [] (int index) {return a[index];}
};

int main () {
  array<int,20> intArray (0); // array<int,20>
  array dblArray (0.0);       // array<double,10> (ab c++17)
  // Compiler leitet anhand von Konstruktorparametern ab:
  // übergebener Datentyp ist double, Default-Wert für SIZE ist 0
}
```

#### Vererbung bei Template-Klassen

- Vererbung zwischen 2 Template-Klassen
  - zulässig, falls die Template-Parameter gleich sind

```c++
template<typename T> class Super {
  public: 
    Super (T t) {}
};
template<typename T> class Sub : public Super<T> {
  public: 
    Sub (T t) : Super<T> (t) {}
};
```

- Vererbung von Template-Klasse an "normale" Klasse
  - zulässig, wenn Template-Parameter explizit angegeben wird

```c++
class SubInt : public Super<int> {    // Hier: Template-Parameter = int
  public: 
    SubInt () : Super<int> (0) {}
};
```

### Variadic Templates

= Klassentemplates / Funktionstemplates mit einer variablen Anzahl an Typ-Parametern 

Beispiel:

```c++
template<typename T> double sum (T v) {
  return v;
}
template<> double sum (const char *p) {
  return std::stod (p);
}
template<typename T, typename... Ts>    // variable Anzahl an Typ-Parametern
  double sum (T v, Ts... rest) {
  return sum (v) + sum (rest...);
}
int main () {
  std::cout << sum (1.2) << "\n";             
  std::cout << sum (1.2, "12", 42) << "\n";
}
```

# Woche 13

## Standard Template Library (STL)

= Bibliothek mit verschiedenen Containerklassen und Algorithmen für Container.

- diese sind alle als Templates realisiert, z.B. `vector<int>`
- für geordnete Container *muss* der `operator<` der Klasse überladen sein!

[Übersicht über die verschiedenen Container](http://cplusplus.com/reference/stl/)

### Iteratoren

Die interne Speicherung innerhalb der versch. Containerklassen ist unterschiedlich, weshalb wir Iteratoren zum Durchlaufen von Containern nutzen können:

Es gibt die folgenden Methoden bei *allen* Containern, um einen Iterator zu erhalten:

- `.begin()` liefert einen Iterator der auf das **1. Element** des Containers zeigt
- `.end()` liefert einen Iterator der auf die Position **nach dem letzten Element** des Containers zeigt
- `.cbegin()` / `.cend()` liefert jeweils einen `const` Iterator (dieser kann die Element des Containers **nicht** verändern

Wir können wie folgt durch einen Container iterieren:

```c++
// mit while
  vector<int> v;
  auto it = v.begin ();   // Iterator auf das 1. Element
  while (it != end ())    // solange das Ende (also das ELement _nach_ dem letzten Element) erreicht wurde
  {
    int x = *it;          // mit dem (dereferenzierten 
    it++;                 // Iterator auf das nächste Element setzen (it += 4 würde z.B. auch gehen)
  }

// mit for
  for (auto it = v.begin(); it != v.end(); ++it)
    int x = *it;
```

Sobald wir für eine Klasse `begin()` und `end()` definiert haben können wir auch range-based loops verwenden: `for (char c : string)`

### std::initializer_list

Die meisten Container besitzen einen Konstruktor mit einem Parameter vom Typ `std::initializer_list`

Beim Erzeugen eines Containers mit einer Initialisierungsliste, also z.B. `vector<int> v {1,2,3,4}`, wird **vorrangig** der Konstruktor mit dem Parameter vom Typ `std::initializer_list` ausgeführt.

Beispiel für eine eigene Container-Klasse mit einem solchen Container:

```c++
int *array;
IntArray (const std::initializer_list<int> &list) : array (new int [list.size ()])
  {
    std::copy (list.begin (), list.end (), array);
  }
```

### Übersicht über die wichtigsten Container in VL 10

### Algorithmen

Die STL enthält verschiedene Template-Funktionen für Standard-Anwendungen wie Sortieren, Suchen, Prüfen, Zählen, Kopieren etc.

[Übersicht](http://cplusplus.com/reference/algorithm/)

Die Funktionen arbeiten auf einem Bereich \[first,last) (z.B. `(v.begin(), v.end())`) zwischen zwei Iterator-Positionen first und last und können sowohl für C-Arrays `int array [4]` als auch C++-Container eingesetzt werden. 

Viele Funktionen besitzen zusätzliche Prädikate (*Callable Objects*, also Funktionspointer/Funktoren/Lamdba-Ausdrücke) als Paramater.

#### Auswahl

`vector<int> v {4,7,2,9};` `vector<int> w {}`

- `sort (v.begin(), v.end())` 
- `copy_if (v.begin(), v.end(), w.begin(), compare)`
- `count_if (v.begin(), v.end(), compare)`
- `for_each (v.begin(), v.end(), function)`
