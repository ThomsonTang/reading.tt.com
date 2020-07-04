# The Factory Pattern

## Thinking about **new**

### When you see **new**, think **concrete**

For example, we want to use **interfaces** to keep code flexible, but we have to create an instance of a **concrete** class:

```java
Duck duck = new MallardDuck();
```

or we have a bunch of different duck classes, and we don’t know until runtime which one we need to instantiate.

```java
Duck duck;
if (picnic) {
	duck = new MallardDuck();
} else if (hunting) {
	duck = new DecoyDuck();
} else if (inBathTub) {
	duck = new RubberDuck();
}
```

### What’s wrong with **new**

> designs should be **open for extension but closed for modification**. Remember, our first principle deals with change and guides us to **identify the aspects that vary and separate them from what stays the same**.

## Identify what varies

### identifying the aspects that vary

```java
BeefNoodle orderBeefNoodle() {
	// for flexibility, we really want this to be an abstract class or interface, but we can't directly instantiate either of those.
	BeefNoodle noodle = new BeefNoodle();
	noodle.prepare();
	noodle.make();
	noodle.fill();
	return noodle;
}
```

The code below is **not closed for modification**. If the noodle shop changes its noodle offering, we have to get into this code and modify it.

```java
BeefNoodle orderBeefNoodle(String type) {
        BeefNoodle noodle;
	//this is what varies.  As the noodle type changes, you will have to modify this code.
        if (type.equals("thinner")) {
            noodle = new ThinnerBeefNoodle();
        } else if (type.equals("thin")) {
            noodle = new ThinBeefNoodle();
        } else if (type.equals("thick")) {
            noodle = new ThickBeefNoodle();
        } else {
            noodle = new BeefNoodle();
        }
	//
        noodle.prepare();
        noodle.make();
        noodle.fill();
        return noodle;
    }
```

## Encapsulate object creation

1. Pull the **object creation** code out of the _orderBeefNoodle_ method.
2. Then we place the code in an object that only care about how to create noodles.

```java
public class SimpleNoodleFactory {
	public BeefNoodle createNoodle(String type) {
		BeefNoodle noodle;
		if (type.equals("thinner")) {
            		noodle = new ThinnerBeefNoodle();
        	} else if (type.equals("thin")) {
            		noodle = new ThinBeefNoodle();
        	} else if (type.equals("thick")) {
            		noodle = new ThickBeefNoodle();
        	} else {
            	noodle = new BeefNoodle();
        }
	}
}
```

## Reworking the NoodleShop client class

What we want to do is rely on the factory to create the noodle.

```java
public class BeefNoodleShop {
	SimpleNoodleFactory noodleFactory;
	public BeefNoodleShop(SimpleNoodleFactory noodleFactory) {
		this.noodleFactory = noodleFactory;
	}
}
```
