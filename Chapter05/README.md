# 5 PROLOG Case Study

## Case 1

Write a program to translate formal English into daily English. (materials are extracted from Bratko I., PROLOG: Programming for Artificial Intelligence [2nd ed], Addison-Wesley, 1990.)

For example, changing `we must consider options to transition expeditiously` to `we must study to change fast`.

Firstly represent the phrases to be substituted and their simpler translations as a set of two argument facts:
```prolog
substitution([adversely, impact], [hurt]).
substitution([negatively, impact], [hurt]).
substitution([impact], [affect]).
substitution([transition], [change]).
substitution([consider, options], [learn]).
substitution([evaluate, options], [study]).
substitution([under, advisement], [being, studied]).
substitution([under, consideration], [being, studied]).
```
