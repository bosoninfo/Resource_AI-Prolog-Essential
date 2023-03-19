# Reasoning/AI Programming Part 3

## 4.1 Selection

Selection can be constructed by using two clauses. The basic pattern can be:
```prolog
if_test(X) :- condition(X), then_component(X), !.
if_test(X) :- else_component(X).
```
### *Example 4.1.1*
```prolog
if_test :- condition(X), then_comp, !.
if_test :- else_comp.
condition(X) :- X > 0.
then_comp :- write('positive'), nl.
else_comp :- write('non-positive'), nl.
```
Output
```
?- if_test(0).
non-positive 
true

?- if_test(4).
positive
true
```

### *Example 4.1.2*
```prolog
/* printMenu.pl */

printMenu :- pt, read(X), check(X).
pt :- write('enter a for function a'), nl, write('enter b for function b'), nl.

check(X) :- X == a, start(X), !.
check(X) :- X == b, start(X), !.
check(X) :- printMenu. % return to printMenu if input is not a or b

start(X) :- write('starting function '), write(X), nl.
```

Selection can also be constructed by using the pre-defined predicate `->/0`. The if ... then ... else clause can be defined as such
```prolog
if_test(X) :- (condition(X) -> then_comp(X); else_comp(X)).
```
### *Example 4.1.3*
```prolog
% the example 4.1.1 can also be written as follows
if_test(X) :- (X > 0 -> write('positive'); write('non-positive')), nl.
```

### *Example 4.1.4*
```prolog
/* abstract.pl */
% read a number and output its abstract

abstract :- write('enter X '), read(X), positive(X).
positive(X) :- X < 0, Y is 0 - X, write(Y).
positive(X) :- X >= 0, write(X).
```
Alternatively, you can write the program as
```prolog
abstract :- write('enter X '), read(X), (X < 0 -> Y is 0 - X, write(Y); write(X)).
```
### Excercise 4.1.5
Write a program `voting.pl` that acts as a vote counting machine. It repeatedly reads people’s vote (1 for “support” and 1 for “against”). The counting is terminated when 0 is entered. It, then, displays the numbers of support votes and against votes.
