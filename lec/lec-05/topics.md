# Topics

## Important Points

### Java Casting

> Regarding the casting, [CS2030S](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-04-exception-and-wrapper-classes/diagnostic-quiz#id-02.-type-casting-and-compile-time-error) has introduced two very useful rules on deciding the Run Time Error and Compile Time Error. The [tricky problem in CS2113](https://wenbo-notes.gitbook.io/cs2113-notes/lec/lec-04#a-tricky-problem) also gives you a glimpse of how to apply the rules.

From the topics, there are some important rules

{% stepper %}
{% step %}
#### **RunTime Behavior**

Due to polymorphism, the behavior of the object will reflect the actual type of the object irrespective of the type of the variable holding a reference to it. For example,

{% code lineNumbers="true" %}
```java
// Some codeAnimal a = new DomesticCat(); //implicit upcast
a.speak();
Cat c = (Cat)a; //downcast
c.speak();
DomesticCat dc = (DomesticCat)a; //downcast
dc.speak();

// Output
// I'm a DomesticCat
// I'm a DomesticCat
// I'm a DomesticCat
```
{% endcode %}

{% hint style="success" %}
#### Code Explanation

The call to the `speak()` method in the code below always executes the `speak()` method of the `DomesticCat` class because the actual type of the object remains `DomesticCat` although the reference to it is being downcast/upcast to various other types.
{% endhint %}
{% endstep %}

{% step %}
#### Casting error only in run-time

> As we have seen in [CS2030S](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-04-exception-and-wrapper-classes#exceptions-are-always-triggered-at-run-time), exceptions are always triggered at run-time!

Casting to an incompatible type can result in a `ClassCastException` at **runtime**.
{% endstep %}

{% step %}
#### The use of `instanceof`

We can use the `instanceof` operator to check if a **cast** is safe to perform. For example,

{% code lineNumbers="true" %}
```java
Cat c;
if (a instanceof Cat){
    c = (Cat)a;
}
```
{% endcode %}

{% hint style="success" %}
#### Code Explanation

This code checks if the object `a` is an instance of the `Cat` class before casting it to a `Cat`.
{% endhint %}
{% endstep %}
{% endstepper %}

### Java Abstract Classes

> In CS2030S, I have provided a quite solid [motivation for Java Abstract Class](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-03-polymorphism#abstract-class), feel free to go back and take a look.

Here, there are some importants points regarding Java Abstract Class,

{% stepper %}
{% step %}
#### A class that has an **abstract method** becomes an **abstract class**.

This is because the class definition is incomplete (due to the missing method body) and it is not possible to create objects using an incomplete class definition.
{% endstep %}

{% step %}
#### An abstract class can have 0 abstract method

In Java, even a class that **does not** have any abstract methods _can_ be declared as an abstract class.
{% endstep %}

{% step %}
#### Subclass of an abstract class

There are two possibilities for the subclasse of an abstract class, it can be

1. another abstract class
2. a [concrete class](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-03-polymorphism#concrete-class)

For example,

{% code lineNumbers="true" %}
```java
public abstract class Feline extends Animal {
    public Feline(String name) {
        super(name);
    }

}

public class DomesticCat extends Feline {
    public DomesticCat(String name) {
        super(name);
    }

    @Override
    public String speak() {
        return "Meow";
    }
}
```
{% endcode %}

{% hint style="success" %}
#### Code Explanation

1. The `Feline` class below inherits from the abstract class `Animal` but it does not provide an implementation for the abstract method `speak`. As a result, the `Feline` class needs to be abstract too.
2. The `DomesticCat` class inherits the abstract `Feline` class and provides the implementation for the abstract method `speak`. As a result, it need not be (but _can_ be) declared as abstract.
3. Thus, `Animal a = new Feline("Mittens");` will generate a **compile-error** while `Animal a = new DomesticCat("Mittens");` is okay!
{% endhint %}
{% endstep %}
{% endstepper %}

### Java Interfaces

> Again, the [motivation for Java Intefaces](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-03-polymorphism#interface) has been given in CS2030S in detail! Strongly advised to go visit them again!

Here, only some extra points are noted,

{% stepper %}
{% step %}
#### Interface inherits from other interfaces

**Interfaces can inherit from other interfaces** using the `extends` keyword, similar to a class inheriting another. For example,

{% code lineNumbers="true" %}
```java
public interface SelfDrivableVehicle extends DrivableVehicle {
   void goToAutoPilotMode();
}
```
{% endcode %}

{% hint style="success" %}
#### Code Explanation

Note that the method signatures have no braces and are terminated with a semicolon.
{% endhint %}
{% endstep %}

{% step %}
**Multiple inheritance among interfaces**

A Java interface can inherit multiple other interfaces. **A Java class can&#x20;**_**implement**_**&#x20;multiple interfaces** (and inherit from one class).

{% hint style="danger" %}
An interface **cannot** extend from another class!
{% endhint %}
{% endstep %}

{% step %}
**Interfaces can also contain constants and static methods**.

All the fields in the interface are implicitly declared to be `public static final` (constants). For example,

{% code lineNumbers="true" %}
```java
public interface DrivableVehicle {

    int MAX_SPEED = 150; // A constant

    static boolean isSpeedAllowed(int speed){
        return speed <= MAX_SPEED;
    }

    void turn(Direction direction);
    void changeLanes(Direction direction);
    void signalTurn(Direction direction, boolean signalOn);
    // more method signatures
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
