# Loops

## The standard `for` loop

The standard `for` loop in JavaScript has the syntax as follows:

{% code lineNumbers="true" %}
```javascript
for (initializer; condition; final-expression) {
  // code to run
}
```
{% endcode %}

Here we have:

1. The keyword `for`, followed by some parentheses.
2. Inside the parentheses we have three items, separated by semicolons:
   1. An **initializer** — this is usually a variable set to a number, which is incremented to count the number of times the loop has run. It is also sometimes referred to as a **counter variable**.
   2. A **condition** — this defines when the loop should stop looping. This is generally an expression featuring a comparison operator, a test to see if the exit condition has been met.
   3. A **final-expression** — this is always evaluated (or run) each time the loop has gone through a full iteration. It usually serves to increment (or in some cases decrement) the counter variable, to bring it closer to the point where the condition is no longer `true`.
3. Some curly braces that contain a block of code — this code will be run each time the loop iterates.

### `for...of` loop

To loop through a collection, like [Arrays](../variables/arrays.md), you can use the variation of `for` loop, called `for...of` loop. For example:

{% code lineNumbers="true" %}
```javascript
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (const cat of cats) {
  console.log(cat);
}
```
{% endcode %}

But if you still want to use the standard `for` loop, the code should be as follows:

{% code lineNumbers="true" %}
```javascript
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (let i = 0; i < cats.length; i++) {
  console.log(cats[i]);
}
```
{% endcode %}

Because of the chances that you might forget about the ["edge" of arrays](#user-content-fn-1)[^1], we recommend you to use `for...of` loop to loop through a collocetion, like Arrays.

## `while` and `do...while`

`for` is not the only type of general loop available in JavaScript. There are actually many others and, while you don't need to understand all of these now, it is worth having a look at the structure of a couple of others so that you can recognize the same features at work in a slightly different way.

### `while` loop

First, let's have a look at the [`while`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while) loop. This loop's syntax looks like so:

{% code lineNumbers="true" %}
```javascript
initializer
while (condition) {
  // code to run

  final-expression
}
```
{% endcode %}

This works in a very similar way to the `for` loop, except that the initializer variable is set before the loop, and the final-expression is included inside the loop after the code to run, rather than these two items being included inside the parentheses. The condition is included inside the parentheses, which are preceded by the `while` keyword rather than `for`.

The same three items are still present, and they are still defined in the same order as they are in the for loop. This is because you must have an initializer defined before you can check whether or not the condition is true. The final-expression is then run after the code inside the loop has run (an iteration has been completed), which will only happen if the condition is still true.

### `do...while` loop

The [`do...while`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/do...while) loop is very similar, but provides a variation on the while structure:

{% code lineNumbers="true" %}
```javascript
initializer
do {
  // code to run

  final-expression
} while (condition)
```
{% endcode %}

In this case, the initializer again comes first, before the loop starts. The keyword directly precedes the curly braces containing the code to run and the final expression.

The main difference between a `do...while` loop and a `while` loop is that _the code inside a `do...while` loop is always executed at least once_. That's because the condition comes after the code inside the loop. So we always run that code, then check to see if we need to run it again. In `while` and `for` loops, the check comes first, so the code might never be executed.

## `break` and `continue`

### `break` statement

If you want to exit a loop before all the iterations have been completed, you can use the [break](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break) statement. We already met this in the previous article when we looked at [switch statements](conditionals.md#switch-statements) — when a case is met in a switch statement that matches the input expression, the `break` statement immediately exits the switch statement and moves on to the code after it.

It's the same with loops — a `break` statement will immediately exit the loop and make the browser move on to any code that follows it.

### `continue` statement

The [continue](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/continue) statement works similarly to `break`, but instead of breaking out of the loop entirely, it **skips to the next iteration of the loop**. Let's look at another example that takes a number as an input, and returns only the numbers that are squares of integers (whole numbers).

[^1]: arrays' index starts at 0 and ends at `array.length - 1`.
