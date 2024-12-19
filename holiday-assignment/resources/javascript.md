# JavaScript

> This part mainly contains the notes from the textbook: [Eloquent JavaScript](https://eloquentjavascript.net/).

JavaScript is a _scripting language_ used to create and control dynamic website content, e.g. you can interact directly without doing a page reload for every action.

## JavaScript Basics

### Values, Types, and Operators

#### Numbers

All[^1] numeric values in JavaScript are stored as `number` type. And this type can be used for both integers and floating-point numbers.

<details>

<summary>Special Numbers</summary>

In JavaScript, there are **three** special numbers that are considered numbers but don't behave like normal numbers.

1. `Infinity`: positive infinity
2. `-Infinity`: negative infinity
3. `NaN`: stands for "not a number"

</details>

#### Strings

In JavaScript, _strings_ are written by enclosing their content in quotes (single quotes `'` , double quotes `"` , and backticks `` ` ``)

{% code lineNumbers="true" %}
```javascript
`Down on the sea`
"Lie on the ocean"
'Float on the ocean'
```
{% endcode %}

{% hint style="info" %}
Each character uses 16 bits and uses _Unicode_ standard for mapping.
{% endhint %}

To _concatenate_ a string, we can merely use `+` operator.

{% code lineNumbers="true" %}
```javascript
"con" + "cat" + "e" + "nate"
// This is same as "concatenate"
```
{% endcode %}

Strings written with **single or double quotes** behave very much the same—the only difference lies in which type of quote you need to escape inside of them. **Backtick-quoted strings**, usually called _template literals_, can do a few more tricks. Apart from being able to span lines, they can also embed other values.

{% code lineNumbers="true" %}
```javascript
`half of 100 is ${100 / 2}`
```
{% endcode %}

When you write something inside `${}` in a template literal, its result will be computed, converted to a string, and included at that position. This example produces the string "half of 100 is 50".

{% hint style="info" %}
The `${...}` method will be useful to print strings when combining with `console.log()`.
{% endhint %}

#### Unary Operators

Operators that use one value are called _unary operators_, while those that use two are called _binary operators._

For example, to show the type of a value, we can use the `typeof` _unary operator_.

{% code lineNumbers="true" %}
```javascript
console.log(typeof 4.5)
// → number
console.log(typeof "x")
// → string
```
{% endcode %}

{% hint style="info" %}
`console.log()` is the printing function in JavaScript.
{% endhint %}

#### Boolean values

Like many other programming languages, JavaSciprt has a type called `boolean` and it has two possible values `true` and `false`.

### Program Structure

#### Bindings/Variables

In JavaScript, we use the keyword `let` to define a variable name and `=` to bind a value to it.

{% code lineNumbers="true" %}
```javascript
let ten = 10;
console.log(ten * ten);
// → 100
```
{% endcode %}

{% hint style="info" %}
A single `let` statement may define multiple bindings.

{% code lineNumbers="true" fullWidth="true" %}
```javascript
let one = 1, two = 2;
```
{% endcode %}
{% endhint %}

#### The Environment

The collection of bindings and their values that exist at a given time is called the _environment_.

{% hint style="info" %}
The Environment will be useful in understanding _closure_. But I am not sure whether it will be used in the frontend development, so skipping it first for the sake of time.
{% endhint %}

### Functions

In JavaScript, we can define a function using the style below, where we must use the keyword `function` at the start.

{% code lineNumbers="true" %}
```javascript
function square(x) {
  return x * x;
}
```
{% endcode %}

{% hint style="info" %}
Unlike C, we don't need to specify the type of the return value of a function.
{% endhint %}

#### Arrow Notation

This is an interesting notation and there is an example as follows:

{% code lineNumbers="true" %}
```javascript
const roundTo = (n, step) => {
  let remainder = n % step;
  return n - remainder + (remainder < step / 2 ? 0 : step);
};
```
{% endcode %}

The arrow comes _after_ the list of parameters and is followed by the function’s body. It expresses something like “this input (the parameters) produces this result (the body)”.

## Browser

## Node

[^1]: More precisely speaking, not "all". There is still a type called `BigInt`.
