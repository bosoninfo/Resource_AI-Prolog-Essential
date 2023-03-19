# Reasoning/AI Programming Part 3

## 4.1 Selection

Selection can be constructed by using two clauses. The basic pattern can be:
```prolog
if_test(X) :- condition(X), then_component(X), !.
if_test(X) :- else_component(X).
```
***Example 4.1.1***
```prolog
if_test :- condition(X), then_comp, !.
if_test :- else_comp.
condition(X) :- X > 0.
then_comp :- write('positive'), nl.
else_comp :- write('non-positive'), nl.
```
*Run*
```
?- if_test(0).
non-positive 
true

?- if_test(4).
positive
true
```

***Example 4.1.2***
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
***Example 4.1.3***
```prolog
% the example 4.1.1 can also be written as follows
if_test(X) :- (X > 0 -> write('positive'); write('non-positive')), nl.
```

***Example 4.1.4***
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
**Excercise 4.1.5**

Write a program `voting.pl` that acts as a vote counting machine. It repeatedly reads people’s vote (1 for “support” and -1 for “against”). The counting is terminated when 0 is entered. It, then, displays the numbers of support votes and against votes.
<details>
  <summary>sample answer</summary>
  
  ```prolog
  /* voting.pl */
  voting :- select(0, 0). % select (support, against).
  
  select(S, A) :- input(X), (X = 0 -> terminate(S, A); continue(S, A, X)).
  continue(S, A, X) :- (X > 0 -> S1 is S + 1, select(S1, A); A1 is A + 1, select(S, A1)).
  
  input(X) :- write('type 1 for support or -1 for against or 0 to terminate the voting program '), read(X), nl.
  
  terminate(S, A) :- write(S), write(' support votes and '), write(A), write(' against votes.'), nl.
  ```
</details>

## 4.2 Lists
- A list is a sequence of elements. Each of the elements can be a `constant`, an `atom`, a `list`, or a `structure`.
- The length of a list may vary during the program execution. This means that elements can be inserted/deleted to/from the list.
- Elements are separated by using a comma `,`.
- A list is included in a pair of square brackets.

***Example 4.2.1***
```prolog
% a list of 4 elements
[32, may, [positive(a), positive(b)], [a, b]]

% an empty list (a list with no element)
[]
```
- A list can be divide into two components: the head and the tail.
- The first element of the list is the head; the rest is the tail. The head is a term, and the tail is a list.
- The notation `[|]` is used to separate the head and the tail. If the list contains only one element, then the element makes the head, and the tail is an empty list.

***Example 4.2.2***
```prolog
weekdays([monday, tuesday, wednesday, thursday, friday]).
like([pear]).
```
*Run*
```prolog
?- weekedays(X).
X = [monday, tuesday, wednesday, thursday, friday].

?- weekdays([Frist_day|Rest_of_days]).
First_day = Monday. % a term
Rest_of_days = [tuesday, wednesday, thursday, friday].

?- weekdays([First_day, Second_day|Rest_of_days]).
First_day = Monday.
Second_day = Tuesday.
Rest_of_days = [wednesday, thursday, friday].

?- like([X|Y]).
X = Pear.
Y = [].
```
