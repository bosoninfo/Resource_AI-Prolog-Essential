# 5 PROLOG Case Study

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## Case 1

Write a program to translate formal English into daily English. Materials are extracted from `Bratko I., PROLOG: Programming for Artificial Intelligence [2nd ed], Addison-Wesley, 1990`.

For example, changing `we must consider options to transition expeditiously` to `we must study to change fast`.

- Firstly represent the phrases to be substituted and their simpler translations as a set of two argument facts:
```prolog
substitution([adversely, impact], [hurt]).
substitution([negatively, impact], [hurt]).
substitution([impact], [affect]).
substitution([transition], [change]).
substitution([consider, options], [learn]).
substitution([evaluate, options], [study]).
substitution([under, advisement], [being, studied]).
substitution([under, consideration], [being, studied]).
substitution([expedite], [do]).
substitution([expeditiously], [fast]).
substitution([prioritize], [rank]).
```

- Next, define a predicate that recurses a sentence (a list).
```prolog
convert([], []).
convert(L, NL) :- 
    substituion(S, NS), 
    append(S, L2, L), 
    append(NS, L2, L3), 
    convert(L3, NL), 
    !.
convert([X|L], [X|L2]) :- convert(L, L2).
```

 - The first rule sets the termination condition
 - The second rule does the work of converting.
 - The third rule recurses through the list without changing any words.
 - The program becomes.
 
```prolog
substitution([adversely, impact], [hurt]).
substitution([negatively, impact], [hurt]).
substitution([impact], [affect]).
substitution([transition], [change]).
substitution([consider, options], [learn]).
substitution([evaluate, options], [study]).
substitution([under, advisement], [being, studied]).
substitution([under, consideration], [being, studied]).
substitution([expedite], [do]).
substitution([expeditiously], [fast]).
substitution([prioritize], [rank]).

convert([], []).
convert(L, NL) :- 
    substituion(S, NS), 
    append(S, L2, L), 
    append(NS, L2, L3), 
    convert(L3, NL), 
    !.
convert([X|L], [X|L2]) :- convert(L, L2).

append([], L, L).
append([X|L1], L2, [X|L3]) :- append(L1, L2, L3). 
```

- Once loaded into the PROLOG executor, it can answer queries such as

```
?- convert([we, must, consider, options, to, transition, expeditiously], L).
L = [we, must, learn, to, change, fast]
```

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## Case 2
Farmer, Wolf, Goat, and Cabbage Problem. Materials are extracted from `Luger G.F., Artificial Intelligence: Structures and Strategies
for Complex Problem Solving (6th ed), Addison-Wesley, 2008`.

- A farmer with his wolf, goat and cabbage are at the eastern side of a river and they wish to cross the river.
- They have only one boat and the boat can carry only two items (including the rower) at a time. Only the farmer can row the boat.
- Whenever the farmer is absent, the wolf will eat the goat unless the two animals are in the different sides of the river.
- Similarly, if the goat is left with the cabbage at the absence of the farmer, the goat will eat the cabbage.
- Derive a sequence of crossings of the river so that the four characters arrive safely on the western side of the river.

```prolog
/* predicates for working with list */
reverse_list([]).
reverse_list([H|T]) :- reverse_list(T), write(H), nl.

member(X, [X|_]).
member(X, [_|T]) :- member(X, T).
```
```prolog
/* opposite positions */
opposite(e, w).
opposite(w, e).

/* unsafe states */
unsafe(state(X, Y, Y, _)) :- opposite(X, Y). % wolf eats goat
unsafe(state(X, _, Y, Y)) :- opposite(X, Y). % wolf eats goat
```
```prolog
/* farmer takes wolf to other side */
move(state(X, X, G, C), state(Y, Y, G, C)) :-
    opposite(X, Y),
    not(unsafe(state(Y, Y, G, C))).

/* farmer takes goat to other side */
move(state(X, W, X, C), state(Y, W, Y, C)) :-
    opposite(X, Y),
    not(unsafe(state(Y, W, Y, C))).
```
```prolog
/* farmer takes cabbage to other side */
move(state(X, W, G, X), state(Y, W, G, Y)) :-
    opposite(X, Y), 
    not(unsafe(state(Y, W, G, Y))).

/* farmer takes himself to other side */
move(state(X, W, G, C), state(Y, W, G, C)) :-
    opposite(X, Y),
    not(unsafe(state(Y,W,G,C))).
```
```prolog
/* move to a next state */
path(Goal, Goal, List) :-
    write('Solution path is: '), nl,
    reverse_list(List).

path(State, Goal, List) :-
    move(State, NextState),
    not(member(NextState, List)),
    path(NextState, Goal, [NextState|List]), !.
```

*Run*
```
?- path(state(e, e, e, e), state(w, w, w, w), [state(e, e, e, e)].

Solution Path is:
state(e,e,e,e)
state(w,e,w,e)
state(e,e,w,e)
state(w,w,w,e)
state(e,w,e,e)
state(w,w,e,w)
state(e,w,e,w)
state(w,w,w,w)
true
```
