# Topics

## Important Points

### SWE Sequence Diagrams Basics

**Sequence diagrams** model the interactions between **various entities in a system**, in a specific scenario. Some examples that we can use sequence diagrams:

* To model how components of a system interact with each other to respond to a user action.
* To model how objects inside a component interact with each other to respond to a method call it received from another component.

A **UML sequence diagram** _captures the interactions between multiple entities for a given scenario._ For example,

{% code lineNumbers="true" %}
```java
class Machine {

    Unit producePrototype() {
        Unit prototype = new Unit();
        for (int i = 0; i < 5; i++) {
            prototype.stressTest();
        }
        return prototype;
    }
}

class Unit {

    public void stressTest() {

    }
}
```
{% endcode %}

This code can be converted to the following UML sequence diagram

<figure><img src="../../.gitbook/assets/uml-sequence-diagram-example.png" alt="" width="563"><figcaption></figcaption></figure>

#### UML Notation

<figure><img src="../../.gitbook/assets/sequence-diagram-uml-notation.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="success" %}
### Notes

1. The class/object name is **not underlined** in sequence diagrams.
2. The arrowhead styles depends on the type of method call.
   1. Synchronous method call will use **filled** arrowheads. (In CS2113, we **always** use the **filled** arrowheads, as shown in the example above)
   2. Asynchronous method call will use **lined** arrowheads. (Out of the scope of CS2113, but you must have learned it in CS2030S)
3. Some common notation errors
   1.  Activation bar too long:\


       <figure><img src="../../.gitbook/assets/sequence-notation-common-error-1.png" alt=""><figcaption></figcaption></figure>
   2.  Broken activation bar: When calling the **nested** methods, the outer method shouldn't be **broken**!\


       <figure><img src="../../.gitbook/assets/sequence-notation-common-error-2.png" alt=""><figcaption></figcaption></figure>


{% endhint %}

Now, we sill introduced some detailed notations that will be used when we construct the sequence diagram

{% stepper %}
{% step %}
#### Loops

<figure><img src="../../.gitbook/assets/sequence-diagram-loop-notation.png" alt="" width="338"><figcaption></figcaption></figure>

For example, the `Player` calls the `mark x,y` command or `clear x y` command repeatedly until the game is won or lost.

<figure><img src="../../.gitbook/assets/sequence-diagram-loop-notation-example.png" alt="" width="414"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Object Creation

<figure><img src="../../.gitbook/assets/sequence-diagram-object-creation-notation.png" alt="" width="563"><figcaption></figcaption></figure>

* The arrow that represents the constructor arrives at the side of the box representing the instance.
* The activation bar represents the period the constructor is active.

For example, the `Logic` object creates a `Minefield` object.

<figure><img src="../../.gitbook/assets/sequence-diagram-object-creation-example.png" alt="" width="530"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Minimal Notation

To reduce clutter, **optional elements (e.g, activation bars, return arrows) may be omitted** if the omission does not result in ambiguities or loss of [relevant information](#user-content-fn-1)[^1].
{% endstep %}
{% endstepper %}

### SWE Sequence Diagrams Intermediate

#### Object Deletion

UML uses an `X` at the end of the lifeline of an object to show its deletion.

<figure><img src="../../.gitbook/assets/sequence-diagram-object-deletion-notation.png" alt="" width="472"><figcaption></figcaption></figure>

Although Java doesn't support `delete` operation, we can use the object deletion notation to indicate the point at which the object becomes **ready to be garbage-collected** (e.g., the point at which it ceases to be referenced).

For example, note how `d` lifeline ends with an `X` to show that it is 'deleted' (e.g., ready to be garbage collected) after the `cook()` method returns.

{% code lineNumbers="true" %}
```java
class Chef {
    void cook() {
        Dish d = new Dish();
    }
}
```
{% endcode %}

<figure><img src="../../.gitbook/assets/sequence-diagram-object-deletion-example.png" alt="" width="467"><figcaption></figcaption></figure>

#### Self Invocation

UML can show a method of an object calling another of its own methods.

<figure><img src="../../.gitbook/assets/sequence-diagram-self-invocation-notation.png" alt="" width="307"><figcaption></figcaption></figure>

For example, the `markCellAt(...)` method of a `Logic` object is calling its own `updateState(...)` method.

<figure><img src="../../.gitbook/assets/sequence-diagram-self-invocation-example.png" alt="" width="508"><figcaption></figcaption></figure>

<details>

<summary><strong>A small tip</strong>: 'Unroll' chained/compound method calls before drawing sequence diagram</summary>

Consider the Java statement `new Book().add(new Chapter());`. How do we show it as a sequence diagram?

First, "unroll" it into a simpler series of statements, which can then be drawn as a sequence diagram easily. For example, that statement is equivalent to the following:

{% code lineNumbers="true" %}
```java
Book b = new Book();
Chapter c = new Chapter();
b.add(c);
```
{% endcode %}

And its sequence diagram will look like as follows:

> TODO:

</details>

#### Alternative Paths

UML uses `alt` frames to indicate alternative paths. This can be viewed as the `if/else` branch in the high-level Java code.

<figure><img src="../../.gitbook/assets/sequence-diagram-alternative-paths-notation.png" alt="" width="314"><figcaption></figcaption></figure>

For example, the `Minefield` calls the `Cell#setMine` method if the cell is supposed to be a mined cell, and calls the `Cell:setMineCount(...)` method otherwise.

<figure><img src="../../.gitbook/assets/sequence-diagram-alternative-paths-example.png" alt="" width="406"><figcaption></figcaption></figure>

{% hint style="warning" %}
**No more than one** alternative partitions be executed in an `alt` frame.
{% endhint %}

#### Optional Paths

UML uses `opt` frames to indicate optional paths.

<figure><img src="../../.gitbook/assets/uml-optional-paths-notation.png" alt="" width="305"><figcaption></figcaption></figure>

For example, `Logic#markCellAt(...)` calls `Timer#start()` only if it is the first move of the player.

<figure><img src="../../.gitbook/assets/uml-notation-optional-paths-example.png" alt="" width="421"><figcaption></figcaption></figure>

#### Calls to Static Methods

Method calls to `static` (i.e., class-level) methods are received by the class itself, not an instance of that class. You can use `<<class>>` to show that a participant is the class itself.

For example, `m` calls the static method `Person.getMaxAge()` and also the `setAge()` method of a `Person` object `p`.

<figure><img src="../../.gitbook/assets/unl-call-to-static-methods-examples.png" alt="" width="389"><figcaption></figcaption></figure>

#### Parallel Paths

UML uses `par` frames to indicate parallel paths.

<figure><img src="../../.gitbook/assets/sequence-diagram-parallel-paths-notation.png" alt="" width="314"><figcaption></figcaption></figure>

For example, `Logic` is calling methods `CloudServer#poll()` and `LocalData#poll()` in parallel.

<figure><img src="../../.gitbook/assets/sequence-diagram-parallel-paths.png" alt="" width="535"><figcaption></figcaption></figure>

{% hint style="success" %}
If you show parallel paths in a sequence diagram, the corresponding Java implementation is likely to be _multi-threaded_ because a normal Java program cannot do multiple things at the same time.
{% endhint %}



[^1]: e.g., information relevant to the purpose of the diagram
