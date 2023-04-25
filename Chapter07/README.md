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
- Let `L = {p(X), p(Y)}`.
- Then `θ1 = {X/a, Y/a}`, `θ2 = {X/f(a), Y/f(a)`, `θ3 = {X/Y}` are all unifiers of `L`.
- Let `L = {p(W) ∧ q(Y), p(X) ∧ q(g(c)), p(Z) ∧ q(g(Z))}`.
- Then `θ = {W/c, X/c, Y/g(c), Z/c}` is a unifier of `L`.

### :star: `Definition` Most General Unifier
A unifier `θ` is defined as the most general unifier of `L` if it satisfies the condition that for every unifier `θ1` of `L`, there exists a substitution `φ` of `L`, such that $θ_1 = θ_φ$.
- The most general unifier (mgu) of `L` is the unifier which maximizes the freedom of `L`.
- If `L` has a unifier, then it has a most general unifier.
- Some formulas have no unifiers and therefore they are formulas which cannot be unified. 

***:blue_book: Example 7.4.4***
- `{p(X), p(f(X))}` cannot be unified
- `{p(X) ∧ q(X), p(X) ∨ q(X)}` cannot be unified.
- `{p(X), q(X)}` cannot be unified.

***:blue_book: Example 7.4.5***
- Let `L = {p(X), p(Y)}`.
- Let `θ = {X/Y}`. `θ` is the most general unifier.
- Let `θ1 = {X/a, Y/a}`. `θ1` is a unifier of `L` and `θ1 = θφ`, where `φ = {Y/a}` is a substitution.

### SLD-Derivation
***:blue_book: Example 7.4.5***

Let's again examine the logic program with a goad `← prime_suspect(Who, robbery)`.
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

<img width="1101" alt="image" src="https://user-images.githubusercontent.com/19381768/232371845-dbc078a9-55ad-4f9c-beb5-832354495f21.png">

### :star: `Definition` SLD-Derivation
An SLD-derivation is a process to repeatedly derive a new goal from a logic problem and a given goal with certain computation rule by unification.

Let `P` be a program. Let 

$$G_i : \leftarrow S_1\land\cdots\land S_k\land\cdots\land S_n$$ 

be the $i^{th}$ derived goal and $S_k$ be the subgoal chosen by the computation rule. If there is a clause 

$$A\leftarrow A_1\land\cdots\land A_m$$

in $P$, $S_k$ and $A$ can be unified by $θ_i$ ($θ_i$ is the mgu), then the derived goal is 

$$G_{i+1} :\leftarrow (S_1\land\cdots\land A_1\land\cdots\land A_m\land\cdots\land S_n)θ_i$$

An SLD-derivation is termiated by one of following two conditions
<ol type="a">
    <li>the goal is empty.</li>
    <li>no subgoals can be unified with any heads of the clauses in P.</li>
</ol>

If a SLD-derivation is terminated by (1), the derivation is successful and $θ_0θ_1\cdotsθ_k$ is defined as the answer to the goal. If a SLD-derivation is terminated by (2), the derivation is failed.

- A successful SLD-derivation is also called an SLD-refutation
- Since both sucessful and failed SLD-derivations are derivations with finite steps, they are finite SLD-derivations.
- If a derivation cannot be terminated finitely, it is called an infinite SLD-derivation.

***:blue_book: Example 7.4.6***

Examine the logic program `P`
```
dau(samantha, rebecca) ←
dau(samantha, michael) ←
dau(rebecca, tom) ←
son(jack, michael) ←
male(tom) ←
male(michael) ←
male(jack) ←
parent(rebecca, anna) ←
parent(elizabeth, michael) ←
parent(X, Y) ← son(Y, X)
parent(X, Y) ← dau(Y, X)
father(X, Y) ← parent(X, Y) ∧ male(X)
```
Let the goal be `← son(jack, X) ∧ father(X, jack)`
### :seedling: i = 0
|General|Example|
|--|--|
|Let $G_i :\leftarrow S_1\land S_2\land\cdots\land S_n$ be the goal|`← son(jack, X) ∧ father(X, jack)`|
|and $S_1$ be the subgoal chosen by the computation rule.|`son(jack, X)`|
|If there is a clause $A\leftarrow A_1\land\cdots\land A_m$ is `P`, and $S_1$ and `A` can be unified by $θ_i$ (mgu)|`son(jack, michael) ←` is a clause of the program, and `son(jack, X)` and `son(jack, micheal)` can be unified by `θ_0 = {X/micheal}`|
|then the drived goal is $G_{i+1} :\leftarrow (A_1,\cdots,A_m,S_2,\cdots,S_n)θ_i$|`← father(michael, jack)`|
### :seedling: i = 1
|General|Example|
|--|--|
|Let $G_i :\leftarrow S_1\land S_2\land\cdots\land S_n$ be the goal|`← father(michael, jack)`|
|and $S_1$ be the subgoal chosen by the computation rule.|`father(michael, jack)`|
|If there is a clause $A\leftarrow A_1\land\cdots\land A_m$ is `P`, and $S_1$ and `A` can be unified by $θ_i$ (mgu)|`father(X, Y) ← parent(X, Y) ∧ male(X)` is a clause of the program, and `father(michael, jack)` and `father(X, Y)` can be unified by `θ_1 = {X/micheal, Y/jack}`|
|then the drived goal is $G_{i+1} :\leftarrow (A_1,\cdots,A_m,S_2,\cdots,S_n)θ_i$|`← parent(michael, jack) ∧ male(michael)`|
### :seedling: i = 2
|General|Example|
|--|--|
|Let $G_i :\leftarrow S_1\land S_2\land\cdots\land S_n$ be the goal|`← parent(michael, jack) ∧ male(michael)`|
|and $S_1$ be the subgoal chosen by the computation rule.|`parent(michael, jack)`|
|If there is a clause $A\leftarrow A_1\land\cdots\land A_m$ is `P`, and $S_1$ and `A` can be unified by $θ_i$ (mgu)|`parent(X, Y) ← son(Y, X)` is a clause of the program, and `parent(michael, jack)` and `parent(X, Y)` can be unified by `θ_2 = {X/micheal, Y/jack}`|
|then the drived goal is $G_{i+1} :\leftarrow (A_1,\cdots,A_m,S_2,\cdots,S_n)θ_i$|`← son(jack, michael) ∧ male(michael)`|
### :seedling: i = 3
|General|Example|
|--|--|
|Let $G_i :\leftarrow S_1\land S_2\land\cdots\land S_n$ be the goal|`← son(jack, michael) ∧ male(michael)`|
|and $S_1$ be the subgoal chosen by the computation rule.|`son(jack, michael)`|
|If there is a clause $A\leftarrow A_1\land\cdots\land A_m$ is `P`, and $S_1$ and `A` can be unified by $θ_i$ (mgu)|`son(jack, michael) ←` is a clause of the program, `θ_3 = {}`|
|then the drived goal is $G_{i+1} :\leftarrow (A_1,\cdots,A_m,S_2,\cdots,S_n)θ_i$|`← male(michael)`|
### :seedling: i = 4
|General|Example|
|--|--|
|Let $G_i :\leftarrow S_1\land S_2\land\cdots\land S_n$ be the goal|`← male(michael)`|
|and $S_1$ be the subgoal chosen by the computation rule.|`male(michael)`|
|If there is a clause $A\leftarrow A_1\land\cdots\land A_m$ is `P`, and $S_1$ and `A` can be unified by $θ_i$ (mgu)|`male(michael) ←` is a clause of the program, `θ_4 = {}`|
|then the drived goal is $G_{i+1} :\leftarrow (A_1,\cdots,A_m,S_2,\cdots,S_n)θ_i$|`←`|
|||
|If a SLD-derivation is terminated by (a), we say the derivation is successful.|The SLD-derivation is terminated by the condition (a), so it is successful.|
|$θ_0θ_1\cdotsθ_k$ is defined as the answer to the goal.|`θ_0 = {X/michael}`<br>`θ_1 = {X/michael, Y/jack}`<br>`θ_2 = {X/michael, Y/jack}`<br>`θ_3 = {}`<br>`θ_4 = {}`<br>`θ0θ1θ2θ3θ4 = {X/michael, Y/jack}`|

Note that to prove a goal, we need to find all the successful SLD-derivations.

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## 7.5 SLD-Tree
### :star: `Definition` SLD-Tree
An SLD-tree is a graph that illustrates SLD-derivations to prove a goal. The original goal $G_0$ is the root of the tree. If the goal $G_{i+1}$ is derived from $G_i$, then $G_{i+1}$ is a child of $G_i$. All substitutions are labelled on the tree. A white block represents a successful derivation. A black block represents a failed derivation.

***:blue_book: Example 7.5.1***

Consider the logic program `P`
```
p(a,b) ←
s(a) ←
q(b,b) ←
m(b) ←
p(X,Y) ← s(X) ∧ t(Y)
p(X,Y) ← m(X) ∧ q(X,Y)
q(b,Y) ← s(Y)
```
and the goal `← p(X,Y)`.

1. With the goal `G0: ← p(X,Y)`, `p(X,Y)` is chosen subgoal. Because `p(a,b) ←` is a clause of `P` and the clause can be unified with the chosen subgoal. The SLD-derivation is successful and `θ0 = {X/a, Y/b}` is the unifier.
```mermaid
flowchart TD
    A["p(X,Y)"] --> B["{X/a, Y/b}"] --> C[]
    C:::classwhite
    classDef classwhite fill:#fff
```

2. `G0 : ← p(X,Y)`, `p(X,Y)` is the chosen subgoal. Because `p(X,Y) ← s(X) ∧ t(Y)` is a clause of `P`, the derived goal becomes `G1: ← s(X) ∧ t(Y)` and the unifier `θ0 = {}`.
```mermaid
flowchart TD
    A["p(X,Y)"] --> B["{X/a, Y/b}"]
    B --> C[]
    C:::classwhite
    A --> D["s(X),t(Y)"]
    classDef classwhite fill:#fff
```
- Now `G1: ← s(X) ∧ t(Y)`, `s(X)` is the chosen subgoal. Since `s(a) ←` is a clause of the program and `s(X)` and `s(a)` are unifiable (`θ1 = {X/a}`), the derived goal is `G2: ← t(Y)`.
```mermaid
flowchart TD
    A["p(X,Y)"] --> B["{X/a, Y/b}"]
    B --> C[]
    C:::classwhite
    A --> D["s(X),t(Y)"]
    D --> E["{X/a}"]
    E --> F["t(Y)"]
    classDef classwhite fill:#fff
```
- With `G2: ← t(Y)`, `t(Y)` is the chosen subgoal. As there is no clause that can be unified with `t(Y)`, the derivation is terminated by the condition (b). The derivation is failed.
```mermaid
flowchart TD
    A["p(X,Y)"] --> B["{X/a, Y/b}"]
    B --> C[]
    C:::classwhite
    A --> D["s(X),t(Y)"]
    D --> E["{X/a}"]
    E --> F["t(Y)"]
    F --> G[ ]
    G:::classblack
    classDef classwhite fill:#fff
    classDef classblack fill:#000
```
3. `G0 : ¬p(X,Y)`, ... ..., There are two further successful SLD-derivations. Their unifiers are `θ0 θ1 θ2 = {X/b, Y/b}` and `θ0 θ1 θ2 = {X/b, Y/a}`. Finally, the SLD-tree is
```mermaid
flowchart TD
    A["p(X,Y)"] --> B["{X/a, Y/b}"]
    B --> C[]
    C:::classwhite
    A --> D["s(X),t(Y)"]
    D --> E["{X/a}"]
    E --> F["t(Y)"]
    F --> G[ ]
    F:::classblack
    A --> H["m(X),q(X,Y)"]
    H --> I["{X/b}"]
    I --> J["q(b,Y)"]
    J --> K["{Y/b}"]
    classDef classwhite fill:#fff
    classDef classblack fill:#000
```
