**Problema 1 - Dreptunghiul.**

c) Priviți degradeul din interior. Întrebare: De ce nu este un degrade „neted” pe tot dreptunghiul (secțiunea 5.2)?

Dreptunghiul e format din 2 triunghiuri deci interpolarea este făcută separat pe triunghiuri.

**Problema 2 - Poligonul regulat.**

d) Întrebare: Câte vârfuri și câți indici are un poligon cu n laturi? De ce este necesar % sides la indicele ultimului triunghi? Ce s-ar vedea dacă ați scrie i / sides în loc de (float)i / sides?

vârfuri = n + 1
laturi = n * 3
Conectează ultimul vârf cu vârful 1. ```1 + (1 + sides - 1) % sides = 1```
Varfurile au aceași culoare înafară de centru. (routunjire în jos)

**Problema 3 - Cercul.**
b) Întrebare: Câți pixeli au semiaxele elipsei într-o fereastră 800 × 600? Calculați din formulele x_pixel și y_pixel (secțiunea 4.4). Ce se întâmplă dacă redimensionați fereastra spre o formă pătrată? Dar dacă o lărgiți?

```
x_pixel = ((-0.5NDC + 1) / 2) * 800 = 200px
y_pixel = ((-0.5NDC + 1) / 2) * 600 = 150px
```
Pentru o fereastră pătrată semiaxele devin egale.

c) Întrebare (doar de gândit): Prin ce număr ați înmulți coordonatele x ale vârfurilor ca să obțineți un cerc rotund în fereastra 800 × 600? De ce nu este o soluție bună? (Indiciu: ce se întâmplă la redimensionare?)

Pentru o valoare hardcodată de 0.75 cercul se va deforma la redimensionare.

