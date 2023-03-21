# 1 First Order Logic

## 1.1 Formal Language & Formal System

### :star: Definition - Formal Language
A formal language contains:
- a set of symbols (the alphabet of the language); and
- a set of formation rules determining which sequences of symbols from its alphabet are well formed formulas (wff).

***Example 1.1.1***
```
A formal language can be defined as follows:

symbols {+, #}
formation rule: any sequence of the symbols except for those have two connected #s.
```
Based on the rule,
- `++#++`, `+#++#+++#`, `+++` are wffs of the language
- `+##++`, `+###`, `##` are not wffs.

### Formal Language and Human Languages
|Formal Language|Human Language|
|--|--|
|Examples:<br>First Order Language,Propositional Language|Examples:<br>English, Japanese, Greek|
|alphabet|The set of words and punctuation (e.g. , . ")|
|well formed formulas (wffs)|sentences|
|formation rules|grammar|

Differences between a formal language and a human language
- a sentence in a human language carries a meaning, while a wff in a formal language is abstract unless we assign a meaning to it.
- formation rules in a formal language are precise while grammar in a human language is not.

### :star: Definition - Deductive Apparatus
A deductive apparatus consists of the axioms and rules of inference that can be used to derive theorems.

### :star: Definition - Formal System
A formal language with a deductive apparatus is a formal system.

***Example 1.1.2***
```
Propositional logic, first order logic and higher order logic are formal systems. 
They are called Mathematical logic.
```

### :star: Definition - Semantics and Syntax
A formal language can be studied from two persepectives: semantics and syntax.

The semantics (model theory) of a formal language is the study of the interpretations, truth functions, and models of the language.

The syntax of a formal language (proof theory) is the study of its deductive apparatus.

## 1.2 First Order Logic
First order language is a formal language. First order logic is a formal system.

### :star: Definition: First Order Language
The alphabet of first order language includes:
1. variables (X, Y, Z, ...)
2. constants (a, b, c, ...)
3. function symbols (f, g, ...)
4. predicate symbols (p, q, ...)
5. connectives ($\lnot$ (negation), $\lor$ (disjunction), $\land$ (conjunction), $\leftarrow$ (implication))
6. quantifiers ($\forall$ (universal quantifier), $\exists$ (existing quantifier))
7. punctuation (())

### :star: Term
A term is one of the follows:
- a constant
- a variable
- `f(X1, X2, ..., Xn)` where `f` is an n-ary function symbol and `X1, X2, ..., Xn` are terms.

***Example 1.2.1***
```
If X, Y are variables and a, b are constants then X, b, 20, X^a, sin(Y+b) are terms.
```
### :star: Atom
If p is a predicate symbol and X1, X2, ..., Xn are terms, then p(X1, X2, ..., Xn) is an atom.

### :star: Literal
A literal is an atom or the negation of an atom. A positive literal is an atom and a negative literal is the negation of an atom.

***Example 1.2.2***
> If p is a predicate symbol and a, b, X are terms, then<br>
p(a+b, X, X^a) is an atom.<br>
p(a+b, X, X^a) is also called a positive literal and $\lnot$p(a+b, X, X^a) is a negative literal.
