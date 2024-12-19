# JavaScript

> This part mainly contains the notes from the textbook: [Eloquent JavaScript](https://eloquentjavascript.net/)

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

### Data Structures: Objects and Arrays

#### Arrays

In JavaScript, we use the notation below to create an array

{% code lineNumbers="true" %}
```javascript
let listOfNumbers = [2, 3, 5, 7, 11];
console.log(listOfNumbers[2]);
// → 5
console.log(listOfNumbers[0]);
// → 2
console.log(listOfNumbers[2 - 1]);
// → 3
```
{% endcode %}

#### Properties

Some examples are `myString.length`, which is to get the length of a string.

{% hint style="info" %}
Just like strings,  arrays have a `length` property that tells us how many elements the array has.
{% endhint %}

#### Methods

Both string and array values contain, in addition to the `length` property, a number of properties that hold function values.

{% code lineNumbers="true" %}
```javascript
let doh = "Doh";
console.log(typeof doh.toUpperCase);
// → function
console.log(doh.toUpperCase());
// → DOH
```
{% endcode %}

Properties that contain functions are generally called _methods_ of the value they belong to, as in “`toUpperCase` is a method of a string”.

#### Object

Values of the type _object_ are arbitrary collections of properties. One way to create an object is by using braces as an expression.

{% code lineNumbers="true" %}
```javascript
let day1 = {
  squirrel: false,
  events: ["work", "touched tree", "pizza", "running"]
};
console.log(day1.squirrel);
// → false
console.log(day1.wolf);
// → undefined
day1.wolf = false;
console.log(day1.wolf);
// → false
```
{% endcode %}

The binary `in` operator, when applied to a string and an object, tells you whether that object has a property with that name.

{% code lineNumbers="true" %}
```javascript
let anObject = {left: 1, right: 2};
console.log(anObject.left);
// → 1
delete anObject.left;
console.log(anObject.left);
// → undefined
console.log("left" in anObject);
// → false
console.log("right" in anObject);
// → true
```
{% endcode %}

To find out what properties an object has, you can use the `Object.keys` function. Give the function an object and it will return an array of strings—the object’s property names:

{% code lineNumbers="true" %}
```javascript
console.log(Object.keys({x: 0, y: 0, z: 2}));
// → ["x", "y", "z"]
```
{% endcode %}

## Learning Resources

Besides this textbook, there are many other useful resources:

* [JavaScript at the Mozilla Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)

[^1]: More precisely speaking, not "all". There is still a type called `BigInt`.
