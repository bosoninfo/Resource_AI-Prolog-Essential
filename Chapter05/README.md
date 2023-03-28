# 5 PROLOG Case Study

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## Case 1

Write a program to translate formal English into daily English. (materials are extracted from Bratko I., PROLOG: Programming for Artificial Intelligence [2nd ed], Addison-Wesley, 1990.)

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
Farmer, Wolf, Goat, and Cabbage Problem
