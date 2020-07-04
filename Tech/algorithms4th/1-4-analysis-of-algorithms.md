
## 术语卡——Scientific method
- **术语**：scientific method
- **印象**：The very same approach that scientists use to understand the natural world is effective for studying the running time of programs:
	- **Observe** some feature of the nature world, generally with precise measurements.
	- **Hypothesize** a model that is consistent with the observation.
	- **Predict** events using the hypothesis.
	- **Verify** the predictions by making further observations.
	- **Validate** by repeating until the hypothesis and observation agree.
- **出处**： 《Algorithms 4th》

## 术语卡——Mathematical models
- **术语**： Mathematical models
- **印象**：Knuth’s basic insight is simple: **the total running of a program is determined by two primary factors:**
	- The cost of executing each statement.
	- The frequency of execution of each statement.
- **例子**： The former is a property of the computer, the Java compiler and the operating system; the latter is a property of the program and the input.
- **出处**：[Donald Knuth - Wikipedia][1]

## 术语卡
> - Tilde approximations
> - Order-of-growth classifications

### 印象
- We use **tilde approximations**, where we throw away low-order terms that complicate formulas. We write `~f(N)` to represent any function that when divided by `f(N)` approaches 1 as N grows. We write `g(N) ~ f(N)` to indicate that `g(N)/f(N)`  approaches 1 as N grows.
- Most often, we work with **tilde approximations** of the form `g(N) ~ a f(N)` where `f(N) = N^b log^c N` and refer to `f(N)` as the **The order of growth of g(N)**. We use just a few structural primitives (statements, conditions, loops, nesting, and method calls) to implement algorithms, so very often the order of growth of the cost is one of just a few functions of the problem size N.

### 例子
- Some functions with `tilde approximation` and `order of growth`:
![][image-1]
- Common functions of the problem size N:
![][image-2]


## 金句卡
- No amount of experimentation can ever prove me right; a single experiment can prove me wrong.

[1]:	https://en.wikipedia.org/wiki/Donald_Knuth

[image-1]:	http://algs4.cs.princeton.edu/14analysis/images/tilde.png
[image-2]:	http://algs4.cs.princeton.edu/14analysis/images/classifications.png