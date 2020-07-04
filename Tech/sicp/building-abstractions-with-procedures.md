# Building Abstractions with Procedures

> The acts of the mind, wherein it exerts its power over simple ideas, are chiefly these three:

- Combining several simple ideas into one compound one, and thus all complex ideas are made.
- The second is bring two ideas, whether simple or complex, together, and setting them by one another so as to take a view of them at once, without uniting them into one, by which it gets all its ideas of relations.
- The third is separating them from all other ideas that accompany them in their real existence: this is called abstraction, and thus all its general ideas are made.

> --- John Locke, _An Essay Concerning Human Understanding_ (1690)

## computational process/data/program

**Computational processes** are abstract beings that inhabit computers. As they evolve, process manipulate other abstract things called **data**. The evolution of a process is directly by a pattern of rules called **a program**. People create programs to direct processes.

## 1.1 The elements of Programming

> When we describe a language, we should pay particular attention to the means that the language provides for combing simple ideas to form complex ideas.

Every powerful language has three mechanisms for accomplishing this:

- **primitive expressions**, which represent the simplest entities the language is concerned with.
- **means of combinations**, by which compound elements are built from simpler ones.
- **means of abstraction**, by which compound elements can ben named and manipulated as unit.

Two kinds of elements in programming:

- **data**: data is "stuff" that we want to manipulate.
- **procedures**: procedures are description of the rules for manipulating the data.

### 1.1.1 Expressions

#### Combinations

Expressions such as `(+ 300 46)`, `(/ 10 5)`, formed by delimiting a list of expressions within parentheses in order to denote procedure application, are called **combinations**. The leftmost element in the list is called **operator**, and the other elements are class **operands**. The value of a combination is obtained by applying the procedure specified by the operator to the arguments that are the values of the operands.

#### Prefix notation

Prefix notation have serval advantages:

- it can accommodate procedures that may take an arbitrary number of arguments:

```lisp
(+ 5 4 3 7)
19
(\* 25 4 12)
1200
```

- it extends in a straightforward way to allow combinations to be nested, that is, to have combinations whose elements are themselves combinations:

```lisp
(+ (* 3 5) (- 10 4))
21
```

### 1.1.2 Naming and Environment
