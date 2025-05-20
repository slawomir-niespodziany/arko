## Tematy projektów Intel x86:

1. Z-buffer - rysowanie przecinających się trójkątów, trzy trójkąty, translacja wierzchołków w 3D.
2. Skalowanie obrazu metodą interpolacji dwuliniowej. 
3. Przekształcenie afiniczne na obrazie 2D w czasie rzeczywistym (pochylenie + powiększenie + translacja).
4. Filtr konwolucyjny, w zależności od odległości od wybranego punktu obrazka:
```
        |  0 -1  0 |   |  1  2  1 |
maska = | -1  5 -1 | + |  2 -4  2 | * w, gdzie w = min(1, r / (min(width, height) / 2))
        |  0 -1  0 |   |  1  2  1 | 
```
5. Filtracja obrazu przy pomocy filtru Prewitt`a. Płynnie modyfikowalny kierunek filtracji. 
6. Obroty sceny 3D złożonej z punktów i łączących je linii (bez powierzchni). Rzutowanie perspektywiczne, szkielet czworościanu. 
7. Alpha-blending pomiędzy dwoma obrazami, wg wzoru sinus, wokół punktu wybieranego interaktywnie.
8. Efekt Swirl na wczytanym obrazku. Interaktywnie modyfikowalne parametry.
9. Rzutowanie ortogonalne czworościanu - rózne kolory ścian, obrót w minimum dwóch płaszczyznach, z-buffer.
10. Przekształcenie afiniczne na obrazie 2D w czasie rzeczywistym (obrót + translacja).
11. Fraktal Newton`a f(z) = z^3 - 1 z powiększeniem i translacją (implementacja na liczbach pojedynczej precyzji - float).
12. Fraktal Newton`a f(z) = z^5 - 1 z powiększeniem i translacją  (implementacja na liczbach podwójnej precyzji - double).
13. Rysowanie pięciopunktowej krzywej Beziera.
14. Cieniowanie trójkąta z translacją jego wierzchołków. 
15. Krzywe Lissajous. 
16. Rzutowanie perspektywiczne szkieletu czworościanu - krawędzie o różnych kolorach + obrót w mininimum dwóch płaszczyznach. 




