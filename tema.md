**Problema 1 - Dreptunghiul.**

>c) Priviți degradeul din interior. Întrebare: De ce nu este un degrade „neted” pe tot dreptunghiul (secțiunea 5.2)?

Dreptunghiul e format din 2 triunghiuri deci interpolarea este făcută separat pe triunghiuri.

**Problema 2 - Poligonul regulat.**

>d) Întrebare: Câte vârfuri și câți indici are un poligon cu n laturi? De ce este necesar % sides la indicele ultimului triunghi? Ce s-ar vedea dacă ați scrie i / sides în loc de (float)i / sides?

vârfuri = n + 1
laturi = n * 3
Conectează ultimul vârf cu vârful 1. ```1 + (1 + sides - 1) % sides = 1```
Varfurile au aceași culoare înafară de centru. (routunjire în jos)

**Problema 3 - Cercul.**
>b) Întrebare: Câți pixeli au semiaxele elipsei într-o fereastră 800 × 600? Calculați din formulele x_pixel și y_pixel (secțiunea 4.4). Ce se întâmplă dacă redimensionați fereastra spre o formă pătrată? Dar dacă o lărgiți?

```
x_pixel = ((-0.5NDC + 1) / 2) * 800 = 200px
y_pixel = ((-0.5NDC + 1) / 2) * 600 = 150px
```
Pentru o fereastră pătrată semiaxele devin egale.

>c) Întrebare (doar de gândit): Prin ce număr ați înmulți coordonatele x ale vârfurilor ca să obțineți un cerc rotund în fereastra 800 × 600? De ce nu este o soluție bună? (Indiciu: ce se întâmplă la redimensionare?)

Pentru o valoare hardcodată de 0.75 cercul se va deforma la redimensionare.

**Problema 4 - Culori variabile în timp (HSV).**

>c) Întrebare: De ce nu trebuie să aducem noi _elapsed * 0.05 în intervalul [0, 1)?

Overflow peste 1 aduce valoarea înapoi la 0.

**Problema 5 - Vertex shader, timp și pauză.**

>b) În OnUpdateFrame, în locul marcat TODO (P5b), tratați tasta P (IsKeyPressed) astfel încât să comute _paused, iar _elapsed să crească doar când nu este pauză. Verificați: unda și fundalul îngheață, titlul arată PAUZĂ, numărul de cadre pe secundă rămâne același, iar săgețile funcționează. Întrebare: Ce se oprește la pauză și ce continuă să ruleze?

Am pus pauză doar la calculele care folosesc diferența de timp locală, nu la tot programul, se opresc doar calculele dependente de _elapsed.

>c) În shader.vert, în locul marcat TODO (P5c), calculați culoarea din poziție atunci când uColorMode este 1 (secțiunea 6.4). Apăsați C și comparați pe triunghi, dreptunghi și cerc. Întrebare: Ce culoare are un vârf aflat în (0, 0)? Dar unul aflat în (−1, −1)?

-1, -1 - Albastru
0, 0 - Gri

>d) Schimbați + uTime în - uTime. Întrebare: Încotro se deplasează unda în fiecare caz?

"+" - spre stanga
"-" - spre dreapta

>e) Întrebare: În laboratorul 2 (P5) deformarea se făcea cu BufferSubData, iar acum în vertex shader. Câți octeți ar trebui copiați în GPU la fiecare cadru, pentru o formă cu un milion de vârfuri (6 float pe vârf), dacă am deforma pe CPU? Câți octeți trimitem acum pentru aceeași deformare?

pentru CPU:
1.000.000 vârfuri * 6 valori * 4 bytes = 24 MB/frame 

Acum: uTime, uWaveAmplitude, uWaveFrequency, uColorMode = 4B + 4B + 4B + 4B = 16B