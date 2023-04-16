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
