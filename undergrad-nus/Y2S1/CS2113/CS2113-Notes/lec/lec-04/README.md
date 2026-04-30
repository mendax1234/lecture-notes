# Lec 04

## Tips

{% stepper %}
{% step %}
#### Difference between `protected` and `public`

Again, the full explanation is Week 5's content also. Be patient!
{% endstep %}

{% step %}
#### tP UML

In tP's UML, don't need to show the top `Object` class.
{% endstep %}
{% endstepper %}

## Technical Content

{% stepper %}
{% step %}
#### A tricky problem

Given the subtype relationship, which is `Box <: Shape`, and `Circle <: Shape`, but there is no subtype relationship between `Circle` and `Box`, which of the following assignments are OK (here it means won't cause runtime error and compile-time error)?

{% code lineNumbers="true" %}
```java
// Statement 1
Box b0 = new Shape();
// Statement 2
Shape s0 = new Box();
Shape s1 = new Shape();
// Statement 3
Object o1 = (Object) s1;
Object o2 = new Circle();
// Statement 4
Box b1 = (Box) o2;
// Statement 5
Box b2 = (Box) o1;
// Statement 6
Circle c1 = (Circle) o2;
```
{% endcode %}

Statement 2, 3 and 6 are obviously ok, and statement 1 and 4 are obviously not Ok. The only tricky one is statement 5.

Let's recall the rule we have learned in CS2030S.

1. To see whether a code will generate compile-error or not, we **only** see the CTT of the variable and the type casting. The casted type must be the subtype of CTT of L.H.S and should be within the type hierarchy (either subtype of supertype) of the CTT of R.H.S.
2. Run-time error judgment **only** needs us to see the RTT of the variable. We **must** ignore the type casting! So, during the run-time, we can just **ignore** the type casting and see the subtype relationship between the RTT of the L.H.S and R.H.S variable.

So, obviously, statement 5 won't generate a compile-error. Instead, it will generate a **runtime error** because the RTT of o1 is `Shape`, and it is **not a subtype** of `Box`.
{% endstep %}

{% step %}
#### Git branch

This is Week 5's content, but better to know how it works&#x20;
{% endstep %}
{% endstepper %}
