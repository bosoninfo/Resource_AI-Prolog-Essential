# 4 Reasoning/AI Programming Part 3

## 4.1 Selection

Selection can be constructed by using two clauses. The basic pattern can be:
```prolog
if_test(X) :- condition(X), then_component(X), !.
if_test(X) :- else_component(X).
```

***:blue_book: Example 4.1.1***
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

***:blue_book: Example 4.1.2***
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

***:blue_book: Example 4.1.3***

The example 4.1.1 can also be written as follows
```prolog 
if_test(X) :- (X > 0 -> write('positive'); write('non-positive')), nl.
```

***:blue_book: Example 4.1.4***

The program to read a number and output the abstract of the number.
```prolog
/* abstract.pl */
abstract :- write('enter X '), read(X), positive(X).
positive(X) :- X < 0, Y is 0 - X, write(Y).
positive(X) :- X >= 0, write(X).
```

Alternatively, you can write the program as
```prolog
abstract :- write('enter X '), read(X), (X < 0 -> Y is 0 - X, write(Y); write(X)).
```

***:brain: Excercise 4.1.5***

Write a program `voting.pl` that acts as a vote counting machine. It repeatedly reads people’s vote (1 for “support” and -1 for “against”). The counting is terminated when 0 is entered. It, then, displays the numbers of support votes and against votes.
<details>
  <summary>:bulb: sample answer</summary>
  
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

***:blue_book: Example 4.2.1***
```prolog
% a list of 4 elements
[32, may, [positive(a), positive(b)], [a, b]]

% an empty list (a list with no element)
[]
```

- A list can be divide into two components: the head and the tail.
- The first element of the list is the head; the rest is the tail. The head is a term, and the tail is a list.
- The notation `[|]` is used to separate the head and the tail. If the list contains only one element, then the element makes the head, and the tail is an empty list.

***:blue_book: Example 4.2.2***
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

***:blue_book: Example 4.2.3***
```prolog
/* memberL.pl */
memberL(X, [X|_]).
memberL(X, [_|T]) :- memberL(X, T). % we don't need to consider the head.
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

- The pre-built predicate `member/2` functions exactly like memberL/2.

```
?- member(4, [3, 5, 4, 6]).
true

?- member(4, [1, 2, 3]).
false
```

***:blue_book: Example 4.2.4***
```prolog
week_days([monday, tuesday, wednesday, thursday, friday]).
writeL([]). % termination case.
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

***:blue_book: Example 4.2.5***
```prolog
week_days([monday, tuesday, wednesday, thursday, friday]).
lengthL([], 0).
lengthL([_|T], N) :- lengthL(T, N1), N is N1 + 1.
```

*Run*
```
?- week_days(X), lengthL(X, Y).
X = [monday, tuesday, wednesday, thursday, friday],
Y = 5 ;

?- lengthL([[purple, blue], [white, grey]], X).
X = 2 ;
```

`length/2` is a pre-built predicate for calculating length of a list.

***:blue_book: Example 4.2.6***
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
***Explanation for Example 4.2.6***
1. Base case: `maxL([X], X) :- !.`

   This clause handles the case when there is only one element in the list. The maximum element in a single-element list is the element itself. The "cut" operator `!` is used here to prevent backtracking, as no further search is needed once the base case is reached.

2. Recursive case 1: `maxL([X|T], X) :- maxL(T, M), X > M.`

   This clause is true when the first element in the list, `X`, is greater than the maximum element M of the tail `T`. In this case, the maximum element of the entire list is `X`.

   The predicate first calls `maxL(T, M)` recursively to find the maximum element M in the tail of the list `T`. Then it checks whether `X > M`. If this condition is true, `X` is the maximum element of the entire list.

3. Recursive case 2: `maxL([X|T], M) :- maxL(T, M), X =< M.`

   This clause is true when the first element in the list, `X`, is less than or equal to the maximum element M of the tail `T`. In this case, the maximum element of the entire list is `M`.

   Similar to the previous case, the predicate first calls `maxL(T, M)` recursively to find the maximum element `M` in the tail of the list `T`. Then it checks whether `X =< M`. If this condition is true, `M` is the maximum element of the entire list.

The execution of `maxL([3,7,5,2], M)` will proceed as follows:
```
- maxL([3|[7,5,2]], 3) :- maxL([7,5,2], M1), 3 > M1.
    - maxL([7|[5,2]], 7) :- maxL([5,2], M2), 7 > M2.
        - maxL([5|[2]], 5) :- maxL([2], M3), 5 > M3.
            - maxL([2], 2). (Base case)
        - maxL([5,2], 5) (Recursive case 1)
    - maxL([7,5,2], 7) (Recursive case 1)
- maxL([3,7,5,2], 7) (Recursive case 2)
```
The result is `M = 7`, which is the maximum element in the list `[3, 7, 5, 2]`.

***:blue_book: Example 4.2.7***
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
- The pre-built predicate `delete/3` functions exactly the same.

***:blue_book: Example 4.2.8***
```prolog
week_days([monday, tuesday, wednesday, thursday, friday]).
appendL([], L2, L2).
appendL([H|T], L2, [H|L3]) :- appendL(T, L2, L3).
```
`appendL(List1, List2, NewList)` appends `List2` to the end of `List1` and return the result to `NewList`.

|<img width="290" alt="image" src="https://user-images.githubusercontent.com/19381768/227815364-81f163c9-4123-4140-a1dc-76d2ef051ac2.png">|
|:--:|
| List append|

*Run*
```
?- week_days(X), appendL(X, [saturday, sunday], Week).
X = [monday, tuesday, wednesday, thursday, friday],
Week = [monday, tuesday, wednesday, thursday, friday, saturday, sunday] ;
false
```
- The pre-built predicate `forall/2` can be used to traverse a list, and perform an action.

***:blue_book: Example 4.2.9***
```
?- forall(member(X, [purple, blue, green, orange, red]), (write(X), tab(4))).
purple    blue    green   orange    red
true
```

***:brain: Exercise 4.2.10***
Define predicate `reverseL/2`. In `reverseL(X, Y)`, `X` is a list and `Y` is the reverse of `X`.
<details>
  <summary>:bulb: sample answer</summary>
  
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

***:blue_book: Example 4.3.1***
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

***:blue_book: Example 4.3.2***
```prolog
st1('hello').
st2('there').

maxL([X], X).
maxL([X|T], X) :- maxL(T, M), X > M.
maxL([X|T], M) :- maxL(T, M), X =< M.

appendL([], L, L).
appendL([X|L1], L2, [X|L3]) :- appendL(L1, L2, L3).
```

*Run*
```
?- st1(X), st2(Y), appendL(X, Y, Z), name(W, Z).
X = [104, 101, 108, 108, 111],
Y = [116, 104, 101, 114, 101],
Z = [104, 101, 108, 108, 111, 116, 104, 101, 114, 101],
W = hellothere.

?- name(apple, X), name(pear, Y), appendL(X, Y, Z), name(W, Z).
X = [97, 112, 112, 108, 101],
Y = [112, 101, 97, 114],
Z = [97, 112, 112, 108, 101, 112, 101, 97, 114],
W = applepear.

?- st1(X), maxL(X, Y).
X = [104, 101, 108, 108, 111],
Y = 111
```

***:brain: Exercise 4.3.3***
Define the predicate `conver/2`. `convert(X, Y)` converts `X` which is a string of lower-case chars to `Y` which is the string of capitalised chars. (Note that the ASCII codes for 'A' is 65; for 'a' is 97). For example `?- convert('abba', Y)` should return `Y = ABBA`.
<details>
  <summary>:bulb: sample answer</summary>
  
  ```prolog
  /* convert.pl */
  modify([], X, []).
  modify([H|T], X, [NH|NT]) :- NH is H - X, modify(T, X, NT).
  
  convert(S, NS) :- name(S, L), modify(L, 32, NL), name(NS, NL).
  ```
</details>

## 4.4 Structures
- PROLOG does not support global variables. The scope of a variable is within a clause.
- For example, in `positive(X) :- X > 0.` and `greaterThan(X, Y) :- greaterThan(X, Z), greaterThan(Z, Y).`, the `X` in the first clause and the `X` in the second clause are irrelevant.
- Data structures can be built with user-defined predicates to pass value from one clause to the other. A data structure is an atom or the composition of atoms.

***:blue_book: Example 4.4.1***
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

***:blue_book: Example 4.4.2***
```prolog
student(names(may, smith), 21061985, sbcs, 2021).
book(prolog, author(ivan, bratko), category(cs, area(ai, subject(ai_language)))).
```
*Run*
```
?- student(X, Y, Z, 2021).
X = names(may, smith),
Y = 21061985,
Z = sbcs ;

?- book(prolog, X, Y).
X = author(ivan, bratko),
Y = category(cs, area(ai, subject(ai_language))) ;
```

- `functor/3` is a built-in predicate that can be utilized to examine a structure.
- If `X` is a structure, `functor(X, Y, Z)` binds the predicates symbol of `X` to `Y` and the number of arguments of `X` to `Z`.

***:blue_book: Example 4.4.3***
```prolog
book(prolog, author(ivan, bratko), category(cs, area(ai, subject(ai_language)))).
```
*Run*
```
?- book(prolog, X, Y), functor(X, NX, NAX), functor(Y, NY, NAY).
X = author(ivan, bratko),
Y = category(cs, area(ai, subject(ai_language))),
NX = author,
NAX = 2,
NY = category,
NAY = 2 ;
```

- `arg/3` is a built-in predicate which can be used to get an argument from a structure.
- Let `Y` be the structure, `arg(X,Y,Z)` takes the Xth argument of `Y` and instantiated it to `Z`.

***:blue_book: Example 4.4.4***
```
?- arg(3, student(may, smith, 21061985, sbcs, 2021), Z).
Z = 21061985 ;

?- arg(5, student(may, X, 05061985, sbcs, Y), Z).
Y = Z ;
```

- `functor/3` and `arg/3` can be used together to build structures.

***:blue_book: Example 4.4.5***
```
?- functor(X, names, 2), arg(1, X, may), arg(2, X, smith).
X = names(may, smith)

?- functor(X, names, 2), arg(1, X, may), arg(2, X, smith), functor(Y, student, 2), arg(1, Y, X), arg(2, Y, 20).
X = names(may, smith),
Y = student(names(may, smith), 20)
```

***:brain: Exercise 4.4.6***

Write a program that changes the structure `bookL([prolog, cs, ai, 112.95])` into `book(title(prolog), category(cs, area(ai)), price(112.95))`.

<details>
  <summary>:bulb: sample answer</summary>
   
  ```prolog
  /* change.pl */
  
  bookL([prolog, cs, ai, 112.95]).
  
  change(B) :- 
    bookL([X1, X2, X3, X4]),
    functor(A, area, 1), arg(1, A, X3),
    functor(T, title, 1), arg(1, T, X1), 
    functor(C, category, 2), arg(1, C, X2), arg(2, C, A),
    functor(P, price, 1), arg(1, P, X4),
    functor(B, book, 3), arg(1, B, T), arg(2, B, C), arg(3, B, P).
  ```
</details>

## 4.5 Built-in Predicates

- The built-in operator `=..` is used to convert between a list and a structure.
- The left side of the operator is the structure and right side is the list.
- `a(b1, b2, ..., bn) =.. [a, b1, b2, ..., bn]`

***:blue_book: Example 4.5.1***
```
?- person(names(may, smith), 670903) =.. Y.
Y = [person, names(may, smith), 670903] ;

?- X =.. [names, may, smith].
X = names(may, smith) ;
```

- The built-in predicate `call/1` will invoke a system evaluation.
- `call(X)` will put `X` as a goal for the system to evaluate.

- The built-in predicate `clause/2` can be used to search for the clauses satisfying certain conditions.
- `clause(X, Y)` searches all the user-defined clauses existing in the PROLOG executor, matches `X` with the head and `Y` with the body. In the case the clause is a fact, it will assign "true" to `Y`.

***:blue_book: Example 4.5.2***
```prolog
loop(Index, Index) :- body(Index).
loop(Index, End) :- Index < End, body(Index), Nindex is Index + 1, loop(Nindex, End).

body(Index) :- write('In the loop with index = '), write(Index), nl.
```

*Run*
```
?- clause(loop(X, Y), Z).
X = Y, 
Z = body(Y) ;
Z = (X<Y, body(X), _G441 is X+1, loop(_G441, Y)) ;
false

?- clause(body(X), Y).
Y = (write('In the loop with index = '), write(X), nl) ;
false
```

- The built-in predicate `assert/1` add an atom or a clause into the database of the PROLOG executor. 
- All data being asserted will disappear when the PROLOG executor is halt.

***:blue_book: Example 4.5.3***
```
?- assert(rain(yesterday)).
true.

?- assert(rain(today)).
true.

?- rain(X).
X = yesterday ;
X = today.

?- assert(rain(X)).
true.

?- rain(anyDay).
true.
```

- The built-in predicate `retract/1` functions in the opposite way as `assert/1`.
- `retract(X)` deletes the occurrence of `X` in the database of the PROLOG executor.

***:blue_book: Example 4.5.4***
```
?- assert(rain(yesterday)), assert(rain(today)).
true.

?- rain(X).
X = yesterday ;
X = today.

?- retract(rain(X)).
X = yesterday ;
X = today.

?- rain(X).
false.
```
