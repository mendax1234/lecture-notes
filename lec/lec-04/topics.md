# Topics

## Important Points

{% stepper %}
{% step %}
#### Review [Dynamic Binding](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-03-polymorphism#method-invocation) from CS2030S

Basically, review CS2030S again!
{% endstep %}

{% step %}
#### Java Enumeration

**An&#x20;**_**Enumeration**_**&#x20;is a fixed set of values that can be considered as a data type.** An enumeration is often useful when using a regular data type such as `int` or `String` would allow invalid values to be assigned to a variable.

**You can define an enum type by using the `enum` keyword.** Because they are constants, the names of an enum type's fields are in uppercase letters e.g., `FLAG_SUCCESS` by convention.

For example, you want to define an enumeration to represent days of a week. In your `Day.java`, you can define the following,

{% code title="Day.java" lineNumbers="true" %}
```java
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY
}
```
{% endcode %}

&#x20;Java enumerations are objects of their own enum type. So, in this case, each constant in the `enum` (like `SUNDAY`, `MONDAY`, etc.) is actually an **instance of the enum type** (`Day` in our case). Under the hood, the Java compiler creates something like a **class with fixed static final objects**.

So, in our `Main.java`, we can use the enumerations as follows,

{% code title="Main.java" lineNumbers="true" %}
```java
Day today = Day.MONDAY;
Day[] holidays = new Day[]{Day.SATURDAY, Day.SUNDAY};

switch (today) {
case SATURDAY:
case SUNDAY:
    System.out.println("It's the weekend");
    break;
default:
    System.out.println("It's a week day");
}

```
{% endcode %}
{% endstep %}

{% step %}
#### [Regression](https://nus-cs2113-ay2526s1.github.io/website/schedule/week4/topics.html#quality-assurance-testing-regression-testing-what)

When you modify a system, the modification may result in some unintended and undesirable effects on the system. Such an effect is called a _regression_.

_Regression testing_ is the re-testing of the software to detect regressions. The typical way to detect regressions is retesting all related components, even if they had been tested before.
{% endstep %}

{% step %}
#### [Code Quality: Readability](https://nus-cs2113-ay2526s1.github.io/website/schedule/week4/topics.html#w4-6-code-quality-readability)

Here are some tips for this week's code quality task:

1. Avoid long methods: A method is long if it is more than 30 LoC[^1].
2. Avoid deep nesting: An easier way to figure nesting is to see the indentation.
3. [Avoid complicated expressions](https://nus-cs2113-ay2526s1.github.io/website/schedule/week4/topics.html#implementation-code-quality-readability-basic-avoid-complicated-expressions): This often applies in the boolean expressions, e.g., try to give reasonable names for the boolean expression!
4. Avoid magic numbers: Give the magic number a meaningful name.
5. [Avoid premature optimization](https://nus-cs2113-ay2526s1.github.io/website/schedule/week4/topics.html#implementation-code-quality-readability-intermediate-avoid-premature-optimizations): Make it work, make it right, make it fast.
6. Avoid having multiple levels of abstraction within a code fragment: This is guided by the rule called SLAP (Single Level of Abstraction Principle)
{% endstep %}

{% step %}
#### Code Quality: Refactoring

The process of restructuring code in small steps without modifying its external behavior is called _refactoring_.

In short, it is just make your code work first, then improve your code quality step by step.
{% endstep %}
{% endstepper %}

## Classic Questions

{% stepper %}
{% step %}
#### Regression Testing

Regression testing is the automated re-testing of a software after it has been modified.

* [ ] True
* [ ] False
* [x] Partially true

***

It is "Partially true" because regression testing **need not be automated** but automation is **highly recommended.**
{% endstep %}

{% step %}
#### Refactoring and Regresstion Testing

Do you agree with the following statement? Justify your answer.

> Whenever you refactor code to fix bugs, you need not do regression testing if the bug fix was minor.

***

**Disagree**. Because,

1. Even a minor change can have major repercussions on the system. You MUST do regression testing after each change, no matter how minor it is.
2. Fixing bugs is technically not refactoring.
{% endstep %}
{% endstepper %}

[^1]: lines of code.
