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

## 7.3 Logic Programming
In this tutorial, we will concentrate on one type of reasoning – reasoning based on First Order Logic (Logic Programming). In particular, we will learn SLD-resolution which is a top-down approach.

### :star: `Definition` Clause
In first order logic, every formula `F` can be written in the form of

$$(\forall X)(B_1(X)\lor B_2(X)\lor\cdots\lor B_n(X)\leftarrow A_1(X)\land A_2(X)\land\cdots\land A_m(X))$$
