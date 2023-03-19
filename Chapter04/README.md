# Reasoning/AI Programming Part 3

## 4.1 Selection

Selection can be constructed by using two clauses. The basic pattern can be:
```prolog
if_test(X) :- condition(X), then_component(X), !.
if_test(X) :- else_component(X).
```
Code 4.1.1
```prolog
if_test :- condition(X), then_comp, !.
if_test :- else_comp.
condition(X) :- X > 0.
then_comp :- write('positive '), nl.
else_comp :- write('non-positive '), nl.
```
Output 4.1.1
```
?- if_test(0).
non-positive 
true

?- if_test(4).
positive
true
```

Code 4.1.2
```prolog
/* printMenu.pl */

printMenu :- pt, read(X), check(X).
pt :- write('enter a for function a'), nl, write('enter b for function b'), nl.

check(X) :- X == a, start(X), !.
check(X) :- X == b, start(X), !.
check(X) :- printMenu. % return to printMenu if input is not a or b

start(X) :- write('starting function '), write(X), nl.
```
