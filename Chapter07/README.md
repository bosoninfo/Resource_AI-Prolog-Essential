# 7 Reasoning

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## 7.1 Automated Reasoning

### :star: `Definition` Automated Reasoning
Automated reasoning is the study of using computers to simulate the activity of human reasoning. It includes:
- Automated theorem proving (reasoning based on propositional logic)
- Logic programming (reasoning based on first order logic)
- Higher order logic programming (reasoning based on higher order logic)
- Automated abduction (reasoning based on abduction)
- Fuzzy reasoning (reasoning based on fuzzy logic)
- Case-based reasoning
- Frame-based reasoning

Automated Reasoning can be divided into two categories: monotonic and non-monotonic. A reasoning system is monotonic if the set of axioms and the
set of derived consequences will never decrease. Otherwise it
is a non-monotonic system.

***:blue_book: Example 7.1.1***
```
raining
spring
playingTennis
hayFever ← spring
stayingInDoor ← raining ∧ hayFever
playingFootball ← winter
```
- Initially, the fact set is 
- { $\color{cyan}\text{raining, spring, playingTennis}$ }
- From `sping` and `hayFever ← spring`, `hayFever` can be derived, so the fact set becomes
- { $\color{cyan}\text{raining, spring, playingTennis, hayFever}$ }
- From `raining`, `hayFever` and `stayingInDoor ← raining ∧ hayFever`, `stayingInDoor` can be derived, so the fact set becomes
- { $\color{cyan}\text{raining, spring, playingTennis, hayFever, stayingInDoor}$ }
- The fact set has been increasing, and no element has been deleted from it. This type of reasoning is monotomic.

***:blue_book: Example 7.1.2***
```
raining
spring
playingTennis
hayFever ← spring
stayingInDoor ← raining ∧ hayFever
¬playingTennis ← raining
playingFootball ← winter
```
- The initial fact set contains
- { $\color{cyan}\text{raining, spring, playingTennis}$ }
- From `spring` and `hayFever ← spring`, we can derive `hayFever`, so the fact set becomes
- { $\color{cyan}\text{raining, spring, playingTennis, hayFever}$ }
- From `raining`, `hayFever` and `stayingInDoor ← raining ∧ hayFever`, `stayingInDoor` can be derived, so the fact set becomes
- { $\color{cyan}\text{raining, spring, playingTennis, hayFever, stayingInDoor}$ }
- From `raining` and `¬playingTennis ← raining`, we can derive `¬playingTennis`. Therefore, the fact set becomes
- { $\color{cyan}\text{raining, spring, hayFever, stayingInDoor}$ }

The proposition `playingTennis` was, at one stage, in the fact set; but was, later on, deleted. This type of reasoning is non-monotonic – elements in the fact set may be deleted during the derivation.

***:blue_book: Example 7.1.3***
```
likes(alice, chocolate)
likes(alice, wine)
likes(bob, wine)
givesAsPresent(X, Y, Z) ← likes(X, Z) ∧ likes(Y, Z)
```
- From the program, we can conclude a set of facts
- { $\color{cyan}\text{likes(alice, chocolate), likes(alice, wine), likes(bob, wine),  givesAsPresent(alice, bob, wine), givesAsPresent(bob, alice, wine)}$ }

*In Summary*
- ***Example 7.1.1*** and ***Example 7.1.2*** are examples of reasoning based on propositional logic. While ***Example 7.1.3*** is an example of reasoning
based on first order logic.
- ***Example 7.1.1*** and ***Example 7.1.3*** are examples of monotonic reasoning. ***Example 7.1.2*** which includes negation, represents a type of non-monotonic reasoning.

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## 7.2 Automated Reasoning - Top-down and Bottom-up
Reasoning approaches can be divided into two categories: Topdown and Bottom-up.
- Top-down is the approach that starts the derivation from the goal, and traces down to the fact set through rules.
- Bottom-up is the approach to start reasoning from the fact set. It repeatedly applies rules to the fact set, derive new facts, updates the fact set till the goal is proved or the fact set becomes stabilized.

***:blue_book: Example 7.2.1***

With the program in ***Example 7.1.1***, if given a goal `← stayingInDoor`, the derivation (bottom-up) will be as follows.
```
raining
spring
playingTennis
hayFever ← spring
stayingInDoor ← raining ∧ hayFever
playingFootball ← winter
```
- { $\color{cyan}\text{raining, spring, playingTennis}$ }
- { $\color{cyan}\text{raining, spring, playingTennis}$ } + { $\color{orange}\text{hayFever}$ } due to `hayFever ← spring`
- { $\color{cyan}\text{raining, spring, playingTennis, hayFever}$ } + { $\color{orange}\text{stayingInDoor}$ } due to `stayingInDoor ← raining ∧ hayFever`

***:blue_book: Example 7.2.2***

Still with the program in ***Example 7.1.1***, if given a goal `← stayingInDoor`, the top-down derivation will be as follows.
```
raining
spring
playingTennis
hayFever ← spring
stayingInDoor ← raining ∧ hayFever
playingFootball ← winter
```
- To prove `stayingInDoor`, and we know `stayingInDoor ← raining ∧ hayFever`
- All we need to prove is `raining ∧ hayFever`
- `raining` is a fact, all we need to prove is `hayFever`
- To prove `hayFever`, and we know `hayFever ← spring`
- All we need to prove is `spring`
- `spring` is a fact, proved.

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## 7.3 Logic Programming
In this tutorial, we will concentrate on one type of reasoning – reasoning based on First Order Logic (Logic Programming). In particular, we will learn SLD-resolution which is a top-down approach.

### :star: `Definition` Clause
In first order logic, every formula `F` can be written in the form of

$$(\forall X)(B_1(X)\lor B_2(X)\lor\cdots\lor B_n(X)\leftarrow A_1(X)\land A_2(X)\land\cdots\land A_m(X))$$

where $B_i(X)$ and $A_j(X)$ are atoms, and $X$ may be a multi-ary variable. `F` is called a clause when it is in such a form.

Clause `F` is defined as
- empty: if $m=0$ and $n=0$. 
    - $\color{cyan}(\forall X)\leftarrow$
- a unit clause: if $m=0$ and $n=1$. 
    - $\color{cyan}(\forall X)(B_1(X))\leftarrow$
- a definite clause: if $m>0$ and $n=1$. 
    - $\color{cyan}(\forall X)(B_1(X)\leftarrow A_1(X)\land A_2(X)\land\cdots\land A_m(X))$ 
- a goal clause: if $m>0$ and $n=0$.  
    - $\color{cyan}\leftarrow A_1(X)\land A_2(X)\land\cdots\land A_m(X))$

### :star: `Definition` Program Clause, Goal Clause, Horn Clause
A program clause is either a unit clause or a definite clause. Both program clauses and goal clauses are called Horn clauses. Note that since all the variables in a clause are universally quantified, the quantifier can be omitted.

***:blue_book: Example 7.3.1***

Let `a` and `b` be constants; `X` and `Y` be variables; `f(X, Y)` be a function and `r(X)`, `t(Y)`, `p1(X)`, `p2(X)`, `p3(X)`, `q(X)`, `q1(X)` and `q2(X)` be predicates. The following formulas are clauses.
```
q(X) ← p1(X) ∧ p2(X) ∧ p3(X)
r(f(a, b)) ←
← t(Y)
(q1(X) ∨ q2(X)) ← p1(X) ∧ p2(X) ∧ p3(X)
```
But the following formulas are not clauses
```
(∃Y)(r(b) ∨ t(Y))
r(Y) ∧ (∀Y)t(Y)
```

### :star: `Definition` Logic Program
A finite set of program clauses is defined as a logic program (Horn clause logic program), denoted as `P`. In the program, 
- a unit clause is called a fact
- a definite clause is called a rule. 

Note that this is exactly the same as we defined in PROLOG except the notations:
- `¬` is represented as `:-` in PROLOG
- `∧` is written as `,` in PROLOG
- `∨` is written as `;` in PROLOG

***:blue_book: Example 7.3.2***

If `a` and `b` are constants; `X`, `Y`, `Z` are variables; `p(X)`, `q(X)`, `s(X)`, `r(X,Y)` and `t(X,Y)` are predicates, then the following code is a logic program.
```
p(a) ←
q(b) ←
s(a) ←
r(Y,Z) ← p(Y) ∧ q(Z)
t(X,Y) ← r(X,Y) ∧ s(X)
```
- Clauses `p(a) ←`, `q(b) ←`, `s(a) ←` are the facts
- Clauses `r(Y,Z) ← p(Y) ∧ q(Z)`, `t(X,Y) ← r(X,Y) ∧ s(X)` are the rules.

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## 7.4 SLD-Resolution
- SLD-resolution stands for SL-resolution for Definite clauses. 
- SL-resolution stands for Linear resolution with Selection functions.
- SLD-resolution defines the process to prove a goal for a logic program.
- SLD-resolution contains a set of SLD-derivations for inference and a set of rules (computation rule, selection rule, and ordering rule) for selection.

***:blue_book: Example 7.4.1***

Let's examine the logic program with a goad `← prime_suspect(Who, robbery)`.
```
possible_suspect(fred) ←
prime_suspect(jack, robbery) ←
crime(robbery, jo, wednesday, pub) ←
was_at(fred, wednesday, pub) ←
possible_suspect(micheal) ←
was_at(micheal, wednesday, pub) ←
had_motive_against(micheal, jo) ←
prime_suspect(Person, Crime) ← 
    crime(Crime, Victim, Time, Place) ∧
    possible_suspect(Person) ∧ 
    was_at(Person, Time, Place) ∧
    had_motive_against(Person, Victim)
```
- To prove `prime_suspect(Who, robbery)`, all we need to prove is `crime(Crime, Victim, Time, Place) ∧ possible_suspect(Person) ∧ was_at(Person, Time, Place) ∧ had_motive_against(Person, Victim)`
- This replacement of `Person` with `Who`, and `Crime` with `robbery`, is a substitution. It is denoted as `{Person/Who, Crime/robbery}`.

### :star: `Definition` Substitution
A substitution is a binding $X_i$ with $t_i$ ( $i = 1, 2,\cdots, n$ ), where { $X_1, X_2,\cdots, X_n$ } is a set of variables and { $t_1, t_2, \cdots, t_n$ } is a set of terms. A substitution is ground, if $t_i$ is a constant ( $i = 1, 2, \cdots, n$ ).

***:blue_book: Example 7.4.2***
- `p(f(a), T, Z) ∧ q(u, a)` 
- is the result of the substitution of `p(f(X), Y, Z) ∧ q(u, X)` with `θ = {X/a, Y/T}`.
- The formula `p(f(a), b, c) ∧ q(u, a)` 
- is the result of substitution of `p(f(X), Y, Z) ∧ q(u, X)` with `α = {X/a, Y/b, Z/c}`, `α` is a ground substitution.

***:blue_book: Example 7.4.3***
Substituions can be composed.
- Let `θ = {X/a, Y/T}, β = {T/b, Z/c}` be two substituions
- We denote `((p(f(X), Y, Z) ∧ q(u, X))θ)β` as `(p(f(X), Y, Z) ∧ q(u, X))(θβ)`
- `θβ = {X/a, Y/b, Z/c}` is called the composition of substitions `θ` and `β`

### :star: `Definition` Unification
Let $L = \lbrace S_1, S_2,\cdots, S_n \rbrace$ be a set of formulas. A substitution `θ` is defined as a unifier of `L`, if it satisfies $S_1θ = S_2θ = \cdots = S_nθ$. `L` is unifiable, if it has a unifier. The procedure of finding the unifier and unifying `L` is called the unification of `L`. (A set of formulae may have more than one unifier.)

***:blue_book: Example 7.4.3***
- Let `L = {p(X), p(Y)}`
- Then all `θ1 = {X/a, Y/b}`, `2
