# FAQ

### Warum übergibt man im Parameter eine `const int &i` const Referenz?

Aus Gründen der Performance: Würden wir einen "normalen" Parameter übergeben, so müsste dieser kopiert werden.

Mit der Referenz `&` sparen wir uns diese Kopie.

Das `const` sorgt nun noch dafür (und teilt auch dem Compiler mit), dass die übergebene Referenz nicht verändert wird.
