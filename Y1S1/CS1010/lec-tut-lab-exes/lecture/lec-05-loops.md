# Lec 05 - Loops

Slides:

{% embed url="https://nus-cs1010.github.io/2425-s1/slides/cs1010-lec05.html#1" %}
Lecture Slides
{% endembed %}

## Loops

### `for` loop

The `for` loop in C has the following syntax:

{% code lineNumbers="true" %}
```c
for (<initialize>; <condition>; <update>) {
    <body>
}
```
{% endcode %}

### `while` loop

The `while` loop looks like this:

{% code lineNumbers="true" %}
```c
<initialize>
while (<condition>) {
    <body>
    <update>
}
```
{% endcode %}

Note that there is no restriction `<update>` must execute after `<body>` in a `while` loop. But in a `for` loop, `<udpate>` is **always** executed **after** `<body>`.

### `do-while` loop

The `do-while` loop looks similar to the `while` loop, except that the **body of the loop is guaranteed to be executed at least once**.

{% code lineNumbers="true" %}
```c
<initialize>
do {
    <body>
    <update>
} while (<condition>);
```
{% endcode %}

Similar to the `while` loop, the `<body>` component and `<update>` componenet in the loop do not have to be in order.

{% hint style="info" %}
When doing paper questions about the loop, **always think about whether it will become an infinite loop or not**.
{% endhint %}

## Loop Invariant

A _loop invariant_ is an assertion that is true before the loop, after each iteration of the loop, and after the loop.

**Argue that an invariant is true**

* it is true before entering the loop.
* it is true at the end of the first iteration of the loop
* if it is true at the end of the $$k$$-th iteration of the loop, then it is true at the end of the $$k+1$$-th iteration.
* it is true when we exit the loop.

Loop invariant, however, is not unique. We can write down infinitely many loop invariants. A good invariant, however, will lead us to an assertion that we want to see (e.g., relating `product` to `n`). We can derive other invariants in our code (such as `{ n != 0 }` below) that do not contribute to the reasoning of the behavior of our loop. _Such invariants should be avoided_.

{% hint style="info" %}
In the past year final papers, you may see that the _loop invariant_ doesn't need to contain exact expression, it can contain some formative words, like "m is the minimum element in the list", etc.
{% endhint %}
