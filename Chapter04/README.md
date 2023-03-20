# Reasoning/AI Programming Part 3

## 4.1 Selection

Selection can be constructed by using two clauses. The basic pattern can be:
```prolog
if_test(X) :- condition(X), then_component(X), !.
if_test(X) :- else_component(X).
```

***Example 4.1.1***
```prolog
if_test(X) :- condition(X), then_comp, !.
if_test(_) :- else_comp. % actually, we don't need to check the value of X here!
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
check(_) :- printMenu. % return to printMenu if input is not a or b

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
  voting :- select(0, 0). % initialise select (support, against).
  
  select(S, A) :- input(X), (X =:= 0 -> terminate(S, A); continue(S, A, X)).
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
```
?- weekedays(X).
X = [monday, tuesday, wednesday, thursday, friday].

?- weekdays([Frist_day|Rest_of_days]).
First_day = monday. % a term
Rest_of_days = [tuesday, wednesday, thursday, friday].

?- weekdays([First_day, Second_day|Rest_of_days]).
First_day = monday.
Second_day = tuesday.
Rest_of_days = [wednesday, thursday, friday].

?- like([X|Y]).
X = pear.
Y = [].
```

***Example 4.2.3***
```prolog
/* memberL.pl */
memberL(X, [X|T]).
memberL(X, [Y|T]) :- memberL(X, T).
```

*Run*
```
% test if X is an element in the list.
?- memberL(c, [a, b, c, d]).
true

?- memberL(a, [1, 2, 3, 4]).
false

% the available value in a list for a vairable.
?- memberL(X, [a, b, c]).
X = a ;
X = b ;
X = c ;
false

?- memberL(a, [X, b, c]).
X = a ;
false
```

The pre-built predicate `member/2` functions exactly like memberL/2.

```
?- member(4, [3, 5, 4, 6]).
true

?- member(4, [1, 2, 3]).
false
```

***Example 4.2.4***
```prolog
week_days([monday, tuesday, wednesday, thursday, friday]).
writeL([]).
writeL([H|T]) :- write(H), nl, writeL(T).
```

*Run*
```
?- week_days(X), writeL(X).
monday
tuesday
wednesday
thursday
friday
X = [monday, tuesday, wednesday, thursday, friday].
false
```

***Example 4.2.5***
```prolog
week_days([monday, tuesday, wednesday, thursday, friday]).
lengthL([], 0).
lengthL([H|T], N) :- lengthL(T, N1), N is N1 + 1.
```

*Run*
```
?- week_days(X), lengthL(X, Y).
X = [monday, tuesday, wednesday, thursday, friday],
Y = 5 ;
false

?- lengthL([[purple, blue], [white, grey]], X).
X = 2 ;
false
```

`length/2` is a pre-built predicate for calculating length of a list.

***Example 4.2.6***
```prolog
maxL([X], X) :- !.
maxL([X|T], X) :- maxL(T, M), X > M.
maxL([X|T], M) :- maxL(T, M), X =< M.
```

*Run*
```
?- maxL([2, 1, 5, 4, 7], X).
X = 7 ;
false

?- maxL([5, 7, X], 20).
X = 20 ;
false

?- maxL([2,1,5,4,7],20).
false
```

***Example 4.2.7***
```prolog
del([], X, []).
del([X|T], X, Rest) :- del(T, X, Rest).
del([Y|T], X, [Y|Rest]) :- X \= Y, del(T, X, Rest).
```
`del(T, X, R)` deletes the element X from the list `T` and assigns the rest of the list to `R`.

*Run*
```
?- del([b, a, c, a], a, R).
R = [b, c] ;
false
```
The pre-built predicate `delete/3` functions exactly the same.

***Example 4.2.8***
```prolog
week_days([monday, tuesday, wednesday, thursday, friday]).
appendL([], L2, L2).
appendL([H|T], L2, [H|L3]) :- appendL(T, L2, L3).
```
`appendL(List1, List2, NewList)` appends `List2` to the end of `List1` and return the result to `NewList`.

*Run*
```
?- week_days(X), appendL(X, [saturday, sunday], Week).
X = [monday, tuesday, wednesday, thursday, friday],
Week = [monday, tuesday, wednesday, thursday, friday, saturday, sunday] ;
false
```
The pre-built predicate `forall/2` can be used to traverse a list, and perform an action.

***Example 4.2.9***
```
?- forall(member(X, [purple, blue, green, orange, red]), (write(X), tab(4))).
purple    blue    green   orange    red
true
```
**Exercise 4.2.10**
Define predicate `reverseL/2`. In `reverseL(X, Y)`, `X` is a list and `Y` is the reverse of `X`.
<details>
  <summary>sample answer</summary>
  
  ```prolog
  /* reverse.pl */
  appendL([], L2, L2).
  appendL([H|T], L2, [H|L3]) :- appendL(T, L2, L3).
  
  reverseL([X], [X]) :- !.
  reverseL([H|T], R) :- reverseL(T, R1), append(R1, [H], R).
  ```
  
</details>

## 4.3 Strings
- A string is a sequence of symbols encolosed in a pair of single quotation marks.
- A string is stored as a list of ASCII codes in the computer memory. Consequently, list operations can be applied to a string.

***Example 4.3.1***
```prolog
'hello', 'there'
```
In the machine, they are stored as
```
[104, 101, 108, 108, 111]
[116, 104, 101, 114, 101]
```
- A built-in predicate `name/2` can be utilized to convert between a string and a list of ASCII codes.
- The first parameter is a string wthile the second is a list.

## 4.4 Structures
- PROLOG does not support global variables. The scope of a variable is within a clause.
- For example, in `positive(X) :- X > 0.` and `greaterThan(X, Y) :- greaterThan(X, Z), greaterThan(Z, Y).`, the `X` in the first clause and the `X` in the second clause are irrelevant.
- Data structures can be built with user-defined predicates to pass value from one clause to the other. A data structure is an atom or the composition of atoms.

***Example 4.4.1***
```prolog
student(may, smith, 21061985, sbcs, 2021).
book(prolog, ivan, bratko, cs, ai, ai_language).
borrow(21061985, prolog).
```
- `student`, `book`, and `borrow` are structures. 
- They are single-layer data structure (ie. their arguments are terms).
- If the structures are loaded in the prolog executor, we can enter queries as below.

*Run*
```
?- student(may, smith, X, Y, Z).
X = 21061985,
Y = sbcs,
Z = 2021 ;

?- book(U, V, W, cs, ai, X).
U = prolog, 
V = ivan, 
W = bratko, 
X = a_language ;

?- borrow(X, prolog), student(Y, Z, X, _, _).
X = 21061985,
Y = may,
Z = smith ;
```
- We can also build multi-layered structures with user-defined predicates.
- A multi-layered structure is a structure that contains others structures (the composition of atoms).

***Example 4.4.2***
```prolog
student(names(may, smith), 21061985, sbcs, 2021).
book(prolog, author(ivan, bratko), category(cs, area(ai, subject(ai_language)))).
```
*Run*

## Built-in Predicates
