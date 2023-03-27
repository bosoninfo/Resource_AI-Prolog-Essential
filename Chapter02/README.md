# 2 Logic & Reasoning / AI programming Part 1

|Table of Sections|
|--|
|:herb:  [2.1 Introduction of Prolog](https://github.com/bosoninfo/Resource_AI-Prolog-Essential/blob/main/Chapter02/README.md#21-)<br>:herb:  [2.2 Lists]()<br>:herb:  [2.3 Strings]()<br>:herb:  [2.4 Structures]()<br>:herb:  [2.5 Built-in Predicates]()|

<p align="center"><img height="75" src="https://user-images.githubusercontent.com/19381768/227871683-af08b378-b283-470e-8b78-bc05937d585b.png"/></p>

## 2.1 Introductin of Prolog

Prolog is a logic programming language which is widely used in AI for tasks that involves symbolic reasoning and symbolic manipulation. Prolog stands for "PROgramming in LOGic", it is developed and implemented in 1970s.

***:blue_book: Example 2.1.1***

There were 7 people in a family reunion, each person made the following statements about the family relations:
- Rebecca:" Samantha is my daughter."
- Michael :"Jack is my son."
- Samantha:" I am Michael's daughter."
- Anna:"Rebecca is one of my parents."
- Elizabeth:"I am one of Michael's parents."
- Tom: "Rebecca is my daughter."
- Jack: "Rebecca, Samantha, Anna and Elizabeth are female; Michael, Tom and I are male."

Based on the above information, we can write a logic program in Prolog to reflect the statements.

Firstly, a number of predicates can be defined as below :
```prolog
daughter(X, Y) % X is a daughter of Y
son(X, Y) % X is a son of Y
parent(X, Y) % X is a parent of Y
mother (X, Y) % X is the mother of Y
father(X, Y) % X is father of Y
male(X) % X is male

parent(X, Y) :- daughter(Y, X).
parent(X, Y) :- son(Y, X).
mother(X, Y) :- parent(X, Y), not(male(X)).
father(X, Y) :- parent(X, Y), male(X).
```
Secondly, the logic program can be written as below:
```prolog
daughter(samantha, rebecca).
son(jack, michael).
daughter(samantha, michael).
parent(rebecca, anna).
parent(elizabeth, michael).
daughter(rebecca, tom).
male(tom).
male(michael).
male(jack).
```
We can then obtain new information from the program by entering a quuery into the Prolog executor:

*Run*
```
?- father(michael, samantha).
true

?_ mother(rebecca, jack).
false

?_ parent(X, samantha).
X = michael
X = rebecca
```
We can extend the relation and add extra rules such as:
```prolog
grandMother(X, Y) :- mother(X, Z), parent(Z, Y).
grandFather(X, Y) :- father(X, Z), parent(Z, Y).
```
The following queries can be answered:

```
?_ grandMother(X, samantha).
?_ grandFather(X, Y).
```

## 2.1 Basics - Constants, Variables, Terms, Predicates and Atoms

In Prolog, the basic building blocks of a program are constants, variables, terms, predicates, and atoms. Here's a brief description of each of these concepts:

**Constants**: A constant is a fixed value that does not change during the execution of a program. In Prolog, constants can be represented by integers, floating-point numbers, or atoms.

**Variables**: A variable is a placeholder that can be assigned a value during the execution of a program. In Prolog, variables are represented by strings that begin with an uppercase letter or an underscore.

**Terms**: A term is a basic unit of Prolog that can be composed of constants, variables, or other terms. Terms can be used to represent logical expressions and are often used as arguments in predicates.

**Predicates**: A predicate is a logical statement that takes one or more arguments and returns a Boolean value (true or false) depending on whether the statement is true or false. Predicates are defined using a name and a set of arguments, and they can be used to represent rules or relationships between objects.

**Atoms**: An atom is a symbolic name that represents a constant or a predicate. Atoms are used to represent identifiers, keywords, and other symbolic names in Prolog programs.

In Prolog, constants, variables, terms, predicates, and atoms are all used to build programs that represent logical statements and relationships between objects. Understanding these basic building blocks is essential for writing effective Prolog programs.
