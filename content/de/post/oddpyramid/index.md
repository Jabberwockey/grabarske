+++
title = "Die ungerade Pyramide"
author = ["Jens Grabarske"]
date = 2025-05-27T21:21:00+02:00
tags = ["Programmierrätsel", "Artikel", "Programmierung", "Mathematik", "Zahlentheorie"]
draft = false
+++

Es ist ja mittlerweile umstritten, in Bewerbungsgesprächen für Softwareentwickler
Programmieraufgaben zu
stellen, da sie höchstens ein recht verzerrtes Bild von den Programmierfähigkeiten
des Kandidaten widerspiegeln.

Dabei mochte ich es eigentlich sehr, Aufgaben wie diese zu stellen, denn sie sagt
mir, wie der Programmierer denkt, bevor er eine Aufgabe anfängt zu programmieren.
Denn genau diese Fähigkeit entscheidet hier darüber, wie schnell man zu einer
Lösung kommt.


## Aufgabe {#aufgabe}

Die ungeraden Zahlen werden wie folgt zu einer unendlichen Pyramide zusammengestellt:
In der ersten Zeile steht die erste ungerade Zahl, also die 1. In der zweiten die
nächsten zwei ungeraden Zahlen, also die 3 und die 5. In der dritten die nächsten
drei und so weiter.

Schreibe ein Programm, das die Summe der Zahlen in Zeile _i_ berechnet.


## Lösung {#lösung}

Ich mag diese Aufgabe sehr. Sie wirkt zunächst so, als wäre sie umständlich und
kompliziert, bis man einmal für die ersten Zeilen es per Hand durchführt.

Das ergibt dann die folgenden Ergebnisse:

| **Zeile** | **Zahlen**  | **Summe** | **bzw.** |
|-----------|-------------|-----------|----------|
| 1         | 1           | 1         | 1^3      |
| 2         | 3 5         | 8         | 2^3      |
| 3         | 7 9 11      | 27        | 3^3      |
| 4         | 13 15 17 19 | 64        | 4^3      |

Für die Zeile _i_ ist die Lösung scheinbar i^3 - programmiertechnisch also sehr
simpel. Eine Lösung in Python, die auch gleich eine Tabelle mit den ersten
10 Zeilen aufbaut, sähe in Python wie folgt aus:

```python
def solution(i):
         return i ** 3

def nth_odd(n):
         return 2 * n - 1

def sum_i(i):
         return i * (i + 1) // 2

def gen_line(i):
         k = sum_i(i - 1) + 1
         return [nth_odd(k + j) for j in range(i)]

def gen_line_str(i):
         return " ".join(map(str, gen_line(i)))

return [["*Zeile*", "*Zahlen*", "*Ergebnis*"], None] + [[i,gen_line_str(i), solution(i)] for i in range(1, 11)]
```

| **Zeile** | **Zahlen**                         | **Ergebnis** |
|-----------|------------------------------------|--------------|
| 1         | 1                                  | 1            |
| 2         | 3 5                                | 8            |
| 3         | 7 9 11                             | 27           |
| 4         | 13 15 17 19                        | 64           |
| 5         | 21 23 25 27 29                     | 125          |
| 6         | 31 33 35 37 39 41                  | 216          |
| 7         | 43 45 47 49 51 53 55               | 343          |
| 8         | 57 59 61 63 65 67 69 71            | 512          |
| 9         | 73 75 77 79 81 83 85 87 89         | 729          |
| 10        | 91 93 95 97 99 101 103 105 107 109 | 1000         |

Dieser Code enthält auch Methoden um die Tabelle selber zu generieren, etwas,
was gar nicht gefordert ist - genau diese Fragestellung bringt die Kandidaten
daher auch ins Schwitzen, bis man ihnen den freundlichen Hinweis gibt, sich doch
erst einmal die ersten Zahlen anzusehen und zu schauen, ob ihnen die Ergebnisse
bekannt vorkommen.

Was ich nicht erwarte, das ist ein Beweis, dabei ist die Art, wie man die Pyramide
erzeugen kann, bereits der halbe Weg, um diesen Zusammenhang nachzuweisen.

Doch dafür zunächst ein Ausflug in die Zahlentheorie.


## Beweis {#beweis}

Zunächst brauchen wir eine Formel, mit der die Summe aller natürlichen Zahlen
bis einschließlich \\(n\\) berechnet werden kann. Dafür gibt es eine Formel, die
von Gauß entwickelt wurde: \\(\frac{n(n+1)}{2}\\) - hier der Beweis des Zusammenhangs:


### Lemma Gauß {#lemma-gauß}

\\(\begin{equation}
       S : \mathbb{N} \rightarrow \mathbb{N}
    \end{equation}\\)

$   \begin{equation}
      S(n) := &sum;<sub>i=1</sub>^n = 1 + \ldots + n =^? \frac{n \cdot (n + 1)}{2}
   \end{equation} $


#### Beweis durch Induktion {#beweis-durch-induktion}

<!--list-separator-->

-  Induktionsanker

    Für \\(n = 1\\) gilt:

    \begin{equation\*}
    S(1) = \sum\_{i=1}^1 = 1 = \frac{1 \cdot 2}{2} \square
    \end{equation\*}

<!--list-separator-->

-  Induktionsannahme

    Angenommen, der Zusammenhang gilt für alle Zahlen bis
    \\(n - 1\\).

<!--list-separator-->

-  Schritt

    Dann gilt:

    \begin{eqnarray\*}
    S(n) &=& n + S(n-1)\\\\
         &=& n + \frac{(n - 1) \cdot ((n - 1) + 1)}{2}\\\\
         &=& n + \frac{(n - 1) \cdot n}{2}\\\\
         &=& \frac{2n + n^2 - n}{2}\\\\
         &=& \frac{n^2 + n}{2}\\\\
         &=& \frac{n \cdot (n + 1)}{2} \square\\\\
    \end{eqnarray\*}


### Lemma Summe der ersten n ungeraden Zahlen {#lemma-summe-der-ersten-n-ungeraden-zahlen}

Es gibt den folgenden überraschenden Zusammenhang: die Summe
der ersten _n_ ungeraden Zahlen ist immer _n^2_ - sprich, alle
Quadratzahlen lassen sich als Summe der ungeraden Zahlen abbilden:

| **n** | **Zahlen** | **Summe** | **bzw.** |
|-------|------------|-----------|----------|
| 1     | 1          | 1         | 1^2      |
| 2     | 1 3        | 4         | 2^2      |
| 3     | 1 3 5      | 9         | 3^2      |
| 4     | 1 3 5 7    | 16        | 4^2      |


#### Beweis durch vollständige Induktion {#beweis-durch-vollständige-induktion}

<!--list-separator-->

-  Behauptung

        \begin{equation\*}
    T : \mathbb{N} \rightarrow \mathbb{N}
        \end{equation\*}

    \begin{equation\*}
    T(n) := \sum\_{i=1}^n 2i - 1 = 1 + 3 + 5 + \ldots + 2n - 1 =^? n^2
    \end{equation\*}

<!--list-separator-->

-  Anker

    Für \\(n = 1\\) gilt:

    \begin{equation\*}
    T(1) = \sum\_{i=1}^1 2i - 1 = 2 - 1 = 1 = 1^2 \square
    \end{equation\*}

<!--list-separator-->

-  Induktionsannahme

    Angenommen, es gilt für \\(T(n-1)\\)

<!--list-separator-->

-  Induktionsschritt

         \begin{eqnarray\*}
    T(n) &=& \sum\_{i=1}^n 2i - 1 \\\\
         &=& (2n - 1) + \sum\_{i=1}^{n-1} 2i - 1 \\\\
         &=& (2n - 1) + T(n-1)\\\\
         &=& (2n - 1) + (n - 1)^2 \\\\
         &=& n^2 - 2n + 1 + 2n - 1 \\\\
         &=& n^2 \square
         \end{eqnarray\*}


### Beweis ursprünglicher Zusammenhang {#beweis-ursprünglicher-zusammenhang}

Man muss sich einmal vor Augen halten:
Die Summe der Zahlen in Zeile i ist die Summe aller Zahlen bis zur
Zeile i minus der Summe der Zahlen bis zur Zeile i - 1.

Die Anzahl der ungeraden Zahlen bis Zeile i ist genau die Summe
aller natürlichen Zahlen bis i. Damit ergibt sich für unser
Problem folgendes:

\begin{eqnarray\*}
P(i) &=& T(S(i)) - T(S(i-1)) \\\\
     &=& S(i)^2 - S(i-1)^2 \\\\
     &=& \frac{i^2 \cdot (i + 1)^2}{4} - \frac{(i - 1)^2 \cdot i^2}{4} \\\\
     &=& \frac{i^2 \cdot (i^2 + 2i + 1) - (i^2 - 2i + 1) \cdot i^2}{4} \\\\
     &=& \frac{i^4 + 2i^3 + i^2 - i^4 + 2i^3 - i^2}{4} \\\\
     &=& \frac{4i^3}{4}\\\\
     &=& i^3 \square
\end{eqnarray\*}

Und damit ist der Zusammenhang bewiesen.
