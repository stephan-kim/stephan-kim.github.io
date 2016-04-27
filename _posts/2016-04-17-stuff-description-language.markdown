---
layout: post
title:  "Thought Experiment: Goldberg, a Stuff Description Language"
date:   2016-04-17 13:40:22 +0200
categories: language-design
---

I saw a talk at Onward! last year, called ["The Cuneiform Tablets of
2015"](http://2015.onward-conference.org/track/onward2015-essays#event-overview). The
question asked in the talk was how to conserve our programs to future
generations -- how can they be kept executable, even when a language
implementation is lost to history? The discussion also included the
situation where (as someone in the audience stated), "society needs
to be rebooted" after a catastrophic event. The talk obviously
included an attempt of an answer, but I will not go into details. I
will instead write about an idea that the talk sparked:

If we can still "run" the [Antikythera
Mechanism](https://en.wikipedia.org/wiki/Antikythera_mechanism) more
than 2000 years after its construction, that is an achievement in
durability that modern programs can only dream of (and my intuition
tells me that with the advance of human technology, that problem will
only get worse, not better). My tongue in cheek "solution" was to
leave messages in the form of massive concrete structures that should
be durable enough to survive most catastrophic events.

After running with this thought for a bit, I asked myself the
question: **what does a programming language look like that compiles
to mechanical machines?**

# Goldberg

Goldberg is a simple language that has numeric expressions, basic
arithmetic, branching, and mutable state.

    exp ::= <numeric literals>
         |  (expr, .., expr)           (tuple)
         |  exp + exp                  (add)
         |  exp - exp                  (subtract)
         |  exp * exp                  (multiply)
         |  exp / exp                  (divide)
         |  IF exp THEN exp ELSE exp   (branch; a non-zero value is "true")
         |  iden                       (read ref cell)

    stmt ::= VAR iden                  (create named ref cell)
         |   iden := expr              (store ref cell)
         |   stmt; stmt                (sequence)

## Representation of Numbers

A traditional language that runs on an actual computer has it easy:
it uses the number representation favoured by its computer
architecture (some binary number format) and uses the architecture's
instructions to operate on such numbers.

In a machine, a number can take many forms, and the choices we make
here are of course not complete.

1. **Angle:** The angle a disc is rotated represents a numeric value.
   ![aaangular](data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+CjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bD0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgdmVyc2lvbj0iMS4xIiB2aWV3Qm94PSIwIDAgODcuMzQ2NDU3IDk4LjM0NjQ1NyIgd2lkdGg9Ijg3LjM0NjQ1N3B0IiBoZWlnaHQ9Ijk4LjM0NjQ1N3B0IiB4bWxuczpkYz0iaHR0cDovL3B1cmwub3JnL2RjL2VsZW1lbnRzLzEuMS8iPjxtZXRhZGF0YT4gUHJvZHVjZWQgYnkgT21uaUdyYWZmbGUgNi41LjIgPGRjOmRhdGU+MjAxNi0wNC0xNyAxODowNDoyOCArMDAwMDwvZGM6ZGF0ZT48L21ldGFkYXRhPjxkZWZzPjxmb250LWZhY2UgZm9udC1mYW1pbHk9IkhlbHZldGljYSBOZXVlIiBmb250LXNpemU9IjE2IiBwYW5vc2UtMT0iMiAwIDUgMyAwIDAgMCAyIDAgNCIgdW5pdHMtcGVyLWVtPSIxMDAwIiB1bmRlcmxpbmUtcG9zaXRpb249Ii0xMDAiIHVuZGVybGluZS10aGlja25lc3M9IjUwIiBzbG9wZT0iMCIgeC1oZWlnaHQ9IjUxNyIgY2FwLWhlaWdodD0iNzE0IiBhc2NlbnQ9Ijk1MS45OTU4NSIgZGVzY2VudD0iLTIxMi45OTc0NCIgZm9udC13ZWlnaHQ9IjUwMCI+PGZvbnQtZmFjZS1zcmM+PGZvbnQtZmFjZS1uYW1lIG5hbWU9IkhlbHZldGljYU5ldWUiLz48L2ZvbnQtZmFjZS1zcmM+PC9mb250LWZhY2U+PC9kZWZzPjxnIHN0cm9rZT0ibm9uZSIgc3Ryb2tlLW9wYWNpdHk9IjEiIHN0cm9rZS1kYXNoYXJyYXk9Im5vbmUiIGZpbGw9Im5vbmUiIGZpbGwtb3BhY2l0eT0iMSI+PHRpdGxlPkNhbnZhcyAxPC90aXRsZT48cmVjdCBmaWxsPSJ3aGl0ZSIgd2lkdGg9Ijc2NiIgaGVpZ2h0PSI2MjgiLz48Zz48dGl0bGU+TGF5ZXIgMTwvdGl0bGU+PGNpcmNsZSBjeD0iMjcuNzIwNDIiIGN5PSIzMi4wMDc4NzQiIHI9IjE0LjE3MzI1MSIgZmlsbD0id2hpdGUiLz48Y2lyY2xlIGN4PSIyNy43MjA0MiIgY3k9IjMyLjAwNzg3NCIgcj0iMTQuMTczMjUxIiBzdHJva2U9ImJsYWNrIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIHN0cm9rZS13aWR0aD0iMSIvPjxsaW5lIHgxPSIyNy43MjA0MiIgeTE9IjE3LjgzNDY0NiIgeDI9IjI3LjcyMDQyIiB5Mj0iNDYuMTgxMTAyIiBzdHJva2U9ImJsYWNrIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIHN0cm9rZS13aWR0aD0iMSIvPjxsaW5lIHgxPSI0Mi4zOTM2NDgiIHkxPSIzMS41MDc4NzQiIHgyPSIxNC4wNDcxOTEiIHkyPSIzMS41MDc4NzQiIHN0cm9rZT0iYmxhY2siIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCIgc3Ryb2tlLXdpZHRoPSIxIi8+PGxpbmUgeDE9IjM3LjUyNDU1MyIgeTE9IjIxLjgxNDQ4NCIgeDI9IjE3LjQ4MDU4MiIgeTI9IjQxLjg1ODQ1NiIgc3Ryb2tlPSJibGFjayIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIiBzdHJva2Utd2lkdGg9IjEiLz48bGluZSB4MT0iMzguMjMxNjYiIHkxPSI0MS44NTg0NTYiIHgyPSIxOC4xODc2ODgiIHkyPSIyMS44MTQ0ODQiIHN0cm9rZT0iYmxhY2siIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCIgc3Ryb2tlLXdpZHRoPSIxIi8+PHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMjMuMjgyOTIgLjMzMDcwODcpIiBmaWxsPSJibGFjayI+PHRzcGFuIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EgTmV1ZSIgZm9udC1zaXplPSIxNiIgZm9udC13ZWlnaHQ9IjUwMCIgeD0iLjA1MiIgeT0iMTUiIHRleHRMZW5ndGg9IjguODk2Ij4wPC90c3Bhbj48L3RleHQ+PHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMzcuNDU2MTQ4IDYpIiBmaWxsPSJibGFjayI+PHRzcGFuIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EgTmV1ZSIgZm9udC1zaXplPSIxNiIgZm9udC13ZWlnaHQ9IjUwMCIgeD0iLjA1MiIgeT0iMTUiIHRleHRMZW5ndGg9IjguODk2Ij4xPC90c3Bhbj48L3RleHQ+PHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoNDQuMjE4MzM1IDIyLjE3Njg3MSkiIGZpbGw9ImJsYWNrIj48dHNwYW4gZm9udC1mYW1pbHk9IkhlbHZldGljYSBOZXVlIiBmb250LXNpemU9IjE2IiBmb250LXdlaWdodD0iNTAwIiB4PSIuMDUyIiB5PSIxNSIgdGV4dExlbmd0aD0iOC44OTYiPjI8L3RzcGFuPjwvdGV4dD48dGV4dCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgzNi43Mjc1NSAzOS4xODQ3NDUpIiBmaWxsPSJibGFjayI+PHRzcGFuIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EgTmV1ZSIgZm9udC1zaXplPSIxNiIgZm9udC13ZWlnaHQ9IjUwMCIgeD0iLjA1MiIgeT0iMTUiIHRleHRMZW5ndGg9IjguODk2Ij4zPC90c3Bhbj48L3RleHQ+PHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMjEuOTI4MTMgNDUuMzIwNzQpIiBmaWxsPSJibGFjayI+PHRzcGFuIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EgTmV1ZSIgZm9udC1zaXplPSIxNiIgZm9udC13ZWlnaHQ9IjUwMCIgeD0iLjA1MiIgeT0iMTUiIHRleHRMZW5ndGg9IjguODk2Ij40PC90c3Bhbj48L3RleHQ+PHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoNy44MzQ2NDU3IDM2LjYzNDY1NCkiIGZpbGw9ImJsYWNrIj48dHNwYW4gZm9udC1mYW1pbHk9IkhlbHZldGljYSBOZXVlIiBmb250LXNpemU9IjE2IiBmb250LXdlaWdodD0iNTAwIiB4PSIuMDUyIiB5PSIxNSIgdGV4dExlbmd0aD0iOC44OTYiPjU8L3RzcGFuPjwvdGV4dD48dGV4dCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgyLjE2NTM1NDMgMjIuNDYxNDI2KSIgZmlsbD0iYmxhY2siPjx0c3BhbiBmb250LWZhbWlseT0iSGVsdmV0aWNhIE5ldWUiIGZvbnQtc2l6ZT0iMTYiIGZvbnQtd2VpZ2h0PSI1MDAiIHg9Ii4wNTIiIHk9IjE1IiB0ZXh0TGVuZ3RoPSI4Ljg5NiI+NjwvdHNwYW4+PC90ZXh0Pjx0ZXh0IHRyYW5zZm9ybT0idHJhbnNsYXRlKDkuMTA5NjkxIDguMTg1NzkyMykiIGZpbGw9ImJsYWNrIj48dHNwYW4gZm9udC1mYW1pbHk9IkhlbHZldGljYSBOZXVlIiBmb250LXNpemU9IjE2IiBmb250LXdlaWdodD0iNTAwIiB4PSIuMDUyIiB5PSIxNSIgdGV4dExlbmd0aD0iOC44OTYiPjc8L3RzcGFuPjwvdGV4dD48L2c+PC9nPjwvc3ZnPgo=)

2. **Length:** The distance a point is moved, relative to a starting
   position represents a numeric value.
3. ...

These quantities are continuous, but our numbers are not -- so we
quantise the numbers. We pick a numeric range from 0..7, so each
number holds three bits of information.

Since all values are numbers, the type of an expression is simply its
form:

    type ::= ANGLE
          |  LENGTH
          |  ...

## Mechanism

If we have numbers of different types, how can we use them together?
Luckily, we can convert numbers from one format to the other:

