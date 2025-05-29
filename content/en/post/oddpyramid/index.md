+++
title = "The Odd Pyramid"
author = ["Jens Grabarske"]
date = 2025-05-27T21:21:00+02:00
tags = ["Zahlentheorie", "Mathematik", "Programmierrätsel", "Artikel", "programming", "mathematics", "number-theory"]
draft = false
+++

Programming challenges during job interviews are discouraged nowadays, as it gives
a skewed image of the skills of the respective candidate.

But I really liked to give a candidate this particular programming challenge, as
it showed me how the developer thought about problems before he started coding.
For thinking of how the solutions look like before starting to code saves a lot
of time in this case.


## Task {#task}

The odd numbers are aligned as follows to a triangle of infinite height:

In the first row there's the first odd number, 1. In the second row the next
two odd numbers, so 3 and 5. In the third the next three, and so on.

Write a program that provides the sum of all the numbers in the row with the
number _i_.


## Solution {#solution}

I really like this task. It appears to be very complicated and complex to
calculate, until you sum the first couple of lines manually.

That would look like this:

| **Row** | **Numbers** | **Sum** | **resp.** |
|---------|-------------|---------|-----------|
| 1       | 1           | 1       | 1^3       |
| 2       | 3 5         | 8       | 2^3       |
| 3       | 7 9 11      | 27      | 3^3       |
| 4       | 13 15 17 19 | 64      | 4^3       |

For the row with the number \\(i\\) the solution appears to be \\(i^3\\) - a program calculating
this is trivial. Here's a solution in Python that also returns a table with
the first 10 lines:

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

return [["*Row*", "*Numbers*", "*Result*"], None] + [[i,gen_line_str(i), solution(i)] for i in range(1, 11)]
```

This code also contains methods to generate the table, something that the
task actually doesn't ask for. But that is usually the biggest headscratcher
for a prospective candidate until you give him the hint that the solution
may be way easier than he thinks and he should just have a look at what the
numbers look like.

I do not expect people to be able to prove that this is the case, even though
the method to construct the pyramid is actually the key to understanding why
_i^3_ is the correct solution.

But for that we need some number theory.


## Proof {#proof}

First we need a formula for the sum of all natural numbers up until and including a number \\(n\\). That formula has been found by the German mathematician Gauß: \\(\frac{n(n+1)}{2}\\) - let's prove that this is the case:


### Lemma Gauß {#lemma-gauß}

For the function \\(S : \mathbb{N} \rightarrow \mathbb{N}\\) defined as

\\[
    S(n) := \sum\_{i=1}^n = 1 + \ldots + n
    \\]

we want to prove: \\(S(n) = \frac{n \cdot (n + 1)}{2}\\)


#### Proof by induction {#proof-by-induction}

<!--list-separator-->

-  Anchor

    For \\(n = 1\\) :
    \\[
        S(1) = \sum\_{i=1}^1 = 1 = \frac{1 \cdot 2}{2}
        \\]

    which was to be shown.

<!--list-separator-->

-  Assumption

    Let's assume, the property holds for all numbers up to
    \\(n - 1\\).

<!--list-separator-->

-  Step

    Then:

    \\[
    S(n) = n + S(n - 1) = n + \frac{(n - 1) \cdot ((n - 1) + 1)}{2}
    \\]

    \\[
    = \frac{2n + n \cdot (n - 1)}{2}
    \\]

    \\[
    = \frac{2n + n^2 - n}{2}
    \\]

    \\[
    = \frac{n + n^2}{2}
    \\]

    \\[
    = \frac{n \cdot (n + 1)}{2}
    \\]

    which was to be proven.


### Lemma Sum of the first _n_ odd numbers {#lemma-sum-of-the-first-n-odd-numbers}

Here's a surprising fact: the sum of the first \\(n\\) odd numbers is always \\(n^2\\) -
meaning, all square numbers can be formed through the sums of the odd
numbers:

Es gibt den folgenden überraschenden Zusammenhang: die Summe
der ersten \\(n\\) ungeraden Zahlen ist immer \\(n^2\\) - sprich, alle
Quadratzahlen lassen sich als Summe der ungeraden Zahlen abbilden:

| **n** | **Numbers** | **Sum** | **resp.** |
|-------|-------------|---------|-----------|
| 1     | 1           | 1       | 1^2       |
| 2     | 1 3         | 4       | 2^2       |
| 3     | 1 3 5       | 9       | 3^2       |
| 4     | 1 3 5 7     | 16      | 4^2       |


#### Proof by induction {#proof-by-induction}

<!--list-separator-->

-  To prove

    Given the function \\(T : \mathbb{N} \rightarrow \mathbb{N}\\) defined as:

    \\[
          T(n) := \sum\_{i=1}^n 2i - 1 = 1 + 3 + 5 + \ldots + 2n - 1
          \\]

    meaning the sum of the first  \\(n\\) odd numbers. We want to show that \\(T(n) = n^2\\).

<!--list-separator-->

-  Anchor

    For \\(n = 1\\) we have:

    \\[
     T(1) = \sum\_{i=1}^1 (2i - 1) = 2 - 1 = 1
     \\]

    and therefore also \\(T(1) = 1^2\\).

<!--list-separator-->

-  Assumption

    Let's assume, the property is true for all numbers up to \\(n - 1\\).

<!--list-separator-->

-  Step

    As

    \\[
          T(n) = 2n - 1 + T(n-1)
          \\]

    this holds:

    \\[
          T(n) = 2n - 1 + (n - 1)^2
          \\]

    \\[
          = n^2 - 2n + 1 + 2n - 1
          \\]

    \\[
          = n^2
          \\]

    which was to be proven.


## Proof of the solution {#proof-of-the-solution}

Note that the sum of all numbers in row _i_ is equal to the sum of all
numbers including row _i_ minus the sum of all numbers up to row _i - 1_.

The number of odd numbers up until and including row _i_ is exactly the sum
of all odd numbers up to the sum of all natural numbers up to i. For our
problem that means:

\\[
   P(i) = T(S(i)) - T(S(i - 1)) = S(i)^2 - S(i-1)^2
   \\]

After substituting \\(S\\) we have:

\\[
   P(i) = \frac{i² \cdot (i + 1)²}{4} - \frac{i² \cdot (i - 1)²}{4}
   \\]

\\[
   = \frac{i² \cdot ((i² + 2i + 1) - (i² - 2i + 1))}{4}
   \\]

\\[
   = \frac{i² \cdot 4 \cdot i}{4}
   \\]

\\[
   = i³
   \\]

which proves that our solution is correct.
