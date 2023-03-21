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
