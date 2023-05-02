---
title: "Simply Typed Lambda Calculus"
date: 2023-05-02T03:28:50+01:00
draft: false
tags: [ "CS349", "Programming Languages", "Lambda Calculus", "Types", "Notes" ]
---
STLC adds simple typing to Lambda Calculus.

# Syntax
- Variables: {$a..z$}, commonly $x$ first
- Types: $t ::= o | t \rightarrow t$
- Terms: $M ::= x | (\lambda x: t.M) | (M M)$

# Logic rules
- Environment: Associations between variables and values; $E=${$\mathcal{T} = \lambda x y. x, ..., 1 = \lambda f x. f x$}
- Judgement: In environment E, e evaluates to n; $E \vdash e \Rightarrow n$

There are type-wise equivalents.
- Type environment: Associations between variables and types; $H=x_1 : t_1, ..., x_n : t_n$
- Type judgement: In environment H, x is of type t; $H \vdash x \Rightarrow t$

[Return to Index ⟶](/posts/cs349-index/)