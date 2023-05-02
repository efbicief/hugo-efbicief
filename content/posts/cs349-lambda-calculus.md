---
title: "Lambda Calculus"
date: 2023-05-01T04:04:16+01:00
draft: false
tags: [ "CS349", "Programming Languages", "Lambda Calculus", "Notes" ]
---
# Syntax
- Variables: {$a..z$}, commonly $x$ first
- Terms: {$A..Z$}, commonly M first. Either a variable, abstraction or application; respectively, $M ::= x | (\lambda x.M) | (M M)$
  - May also be denoted as $e$, expression: $e ::= x | (\lambda x.e) | (e e)$
- Application associates leftwise, and binds tighter than abstraction.

$\lambda$ *binds* a variable to the *scope* that comes after the dot. In $\lambda x. (\lambda y. M)$, the *scope* of $x$'s *binding* is over $(\lambda y. M)$. Unbound variables are called *free*.

Note: we can omit unnessecary parethesis when associating leftwise. $\lambda x. (\lambda y. M)$ = $\lambda x y. M$

# Substitution
$[N/x]M$ means "replace *free variable* x with N in M". eg:
- $[z/y]\lambda x. y = \lambda x. z$
- $[N/x]\lambda x. M = \lambda x. M$, since x is bound (freshness)
- $[N/x](P Q) = ([N/x]P )([N/x]Q)$, since substitution is distributive

# $\alpha$ Equivalence
If we can convert an expression to another and back using only substitution, they are *$\alpha$ equivalent*.
- $[a/x][b/y][c/z](\lambda x y z. x x y y) = (\lambda a b c. a a b b)$
- $[x/a][y/b][z/c](\lambda a b c. a a b b) = (\lambda x y z. x x y y)$
- $\therefore (\lambda x y z. x x y y) \rightarrow_{\alpha} (\lambda a b c. a a b b)$

# $\beta$ Reduction
"Applying an argument"; substituting an argument to an abstraction.
- $(\lambda x.M )N \longrightarrow [N/x]M$

We say A *evaluates* to B if we can transform A into B through some number of $\beta$ reductions: $A \Rightarrow B \equiv A \longrightarrow^{*} B$.
Sometimes, evaluating results in *divergence*, where evaluation gets stuck in a loop.

# $\eta$ Reduction
For when we can simplify an expression without performing a full $\beta$ reduction: $(\lambda x. Mx) \longrightarrow_{\eta} M$
- $(\lambda y. \lambda x. y x) \longrightarrow_{\eta} (\lambda y. y) \longrightarrow_{\eta} ()$

# Evaluation order
We can choose which order to evaluate redexes.
Assuming $M \Rightarrow M\prime$, $N \Rightarrow N\prime$:
- Eager (Normal order):  Reduce leftmost redex first. Evaluate operands first.
  - $(\lambda x.M )N \Rightarrow (\lambda x.M )N\prime \rightarrow_{\alpha} [N\prime /x]M$
- Lazy (Applicative order): Reduce rightmost redex first. $\beta$ reduce only when nessecary, always $\alpha$ reduce first.
  - $(\lambda x.M )N \rightarrow_{\alpha} [N/x]M$
- "Optimising": Evaluate functions first.
  - $(\lambda x.M )N \Rightarrow (\lambda x.M\prime )N \rightarrow_{\alpha} [N /x]M\prime$

# Normal forms
Recall expression definition: $e ::= x | (\lambda x.e) | (e e)$
Introduce:
 - *Normal form* expression: $E ::= \lambda x.E | x E_1 ... E_n$
 - *Weak normal form* expression: $E ::= \lambda x.e | x E_1 ... E_n$
   - Function "contents" may be any expression
 - *Head normal form* expression: $E ::= \lambda x.E | x e_1 ... e_n$
   - Function "operands" may be any expression
 - *Weak Head normal form*: $E ::= \lambda x.e | x e_1 ... e_n$

# De Bruijn representation
Refer to bound variables by the number of scopes since they were bound.
- $(\lambda x.x (\lambda y.x y x (\lambda z.y z x ))x)$ is represented as $(\lambda 1 (\lambda 2 1 2 (\lambda 2 1 3 )) 1)$
- Free variables use *locally nameless terms*, i.e. the next available number not used anywhere else in the expression
  - $(\lambda x. \lambda y. z x (\lambda u. u x)) (\lambda x. w x) $ becomes $(\lambda \lambda 4 2 (\lambda 1 3)) (\lambda 5 1)$, noting that $z$ and $w$ are free variables

# Combinators
6 important closed (context-free) expressions.
- $I=\lambda x. x$ (Identity)
- $K=\lambda x y. x$ (Constant)
- $Y=\lambda f. (\lambda x. f(x x)) (\lambda x. f(x x))$
- $S=\lambda x y z. x z (y z)$
- $B=\lambda x y z. x (y z)$
- $T=\lambda x y. y x$

*Any* closed lambda expression, including other combinators, can be expressed using only S and K.

# Church encodings
## Booleans
- True: $\mathcal{T} = \lambda x y. x$ (= K combinator); choose first input
- False: $\mathcal{F} = \lambda x y. y$; choose second input
- If: $\mathcal{I} = \lambda p x y. p x y$
  - $\mathcal{I} \mathcal{T} x y \Rightarrow x$
  - $\mathcal{I} \mathcal{F} x y \Rightarrow y$
- And: $\mathcal{A} = \lambda p q. p q p$
- Or: $\mathcal{O} = \lambda p q. p p q$
- Not: $\mathcal{N} = \lambda p a b. p b a$

## Numbers
- $0 = \lambda f x. x$
- $1 = \lambda f x. f x$
- $\mathtt{add} = \lambda i j f x. (i f) (j f x)$ 
  - $\mathtt{add}$ 1 1 $\Rightarrow \lambda f x. f (f x) = 2$

[Return to Index ⟶](/posts/cs349-index/)