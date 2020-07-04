
## 技巧卡
> Design Principle
> Identify the aspects of your application that vary and separate them from what stays the same.

Another way to think about this principle:
**take the parts that vary and encapsulate them, so that later you can alter or extend the parts that vary without affecting those that don’t.**

## 术语卡—— 面向接口编程
- 术语：Program to an interface, not an implementation
- 印象：面向接口编程，而不是实现。这里的接口不是指java中的`interface`，而是表示**超类型**。
	- 「Program to an interface」really means 「program to an supertype」
- 例子：
	- Program to implementation:
		```java
		Dog d = new Dog();
		d.bark();
		```
	- Program to interface:
		```java
		Animal animal = new Dog();
		animal.makeSound();
		```

## 技巧卡—— Has-A can be better than Is-A
> Design Principle
> Favor composition over inheritance.

- Creating system using composition gives you a lot more flexibility.
	- It let you encapsulate a family of algorithms into their own set of classes.
	- It also let you **change behavior at runtime**.

## 金句卡
- Change is constant.
- Java interfaces have no implementation code, so no code reuse.

## 术语卡—— Strategy Pattern
- 术语：策略模式
- 印象：**The Strategy Pattern** defines a family of algorithms, encapsulates each one, and makes them interchangeable, strategy lets the algorithm vary independently from clients that use it.