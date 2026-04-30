# Functions

One essential concept in coding is **functions**, which allow you to store a piece of code that does a single task inside a defined block, and then call that code whenever you need it using a single short command — rather than having to type out the same code multiple times.

## Functions vs. methods

{% hint style="info" %}
This part may involve Object-Oriented Programming concepts.
{% endhint %}

**Functions** that are part of objects are called **methods.**

The built-in code we've made use of so far comes in both forms: **functions** and **methods.** You can check the full list of the built-in functions, as well as the built-in objects and their corresponding methods [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects).

You've also seen a lot of **custom functions** in the course so far — functions defined in your code, not inside the browser. Anytime you saw a custom name with parentheses straight after it, you were using a custom function. In our [random-canvas-circles.html](https://mdn.github.io/learning-area/javascript/building-blocks/loops/random-canvas-circles.html) example (see also the full [source code](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/random-canvas-circles.html)) from our [loops article](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Loops), we included a custom `draw()` function that looked like this:

{% code lineNumbers="true" %}
```javascript
function draw() {
  ctx.clearRect(0, 0, WIDTH, HEIGHT);
  for (let i = 0; i < 100; i++) {
    ctx.beginPath();
    ctx.fillStyle = "rgb(255 0 0 / 50%)";
    ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
    ctx.fill();
  }
}
```
{% endcode %}

This function draws 100 random circles inside a [`<canvas>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) element. Every time we want to do that, we can just invoke the function with this:

{% code lineNumbers="true" %}
```javascript
draw();
```
{% endcode %}

rather than having to write all that code out again every time we want to repeat it. Functions can contain whatever code you like — you can even call other functions from inside functions. The above function for example calls the `random()` function three times, which is defined by the following code:

{% code lineNumbers="true" %}
```javascript
function random(number) {
  return Math.floor(Math.random() * number);
}
```
{% endcode %}

We needed this function because the browser's built-in [`Math.random()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) function only generates a random decimal number between 0 and 1. We wanted a random whole number between 0 and a specified number.

## Function parameters

Some functions require **parameters** to be specified when you are invoking them — these are values that need to be included inside the function parentheses, which it needs to do its job properly.

### Optional parameters

Sometimes parameters are optional — you don't have to specify them. If you don't, the function will generally adopt some kind of default behavior. As an example, the array [`join()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) function's parameter is optional:

{% code lineNumbers="true" %}
```javascript
const myArray = ["I", "love", "chocolate", "frogs"];
const madeAString = myArray.join(" ");
console.log(madeAString);
// returns 'I love chocolate frogs'

const madeAnotherString = myArray.join();
console.log(madeAnotherString);
// returns 'I,love,chocolate,frogs'
```
{% endcode %}

If no parameter is included to specify a joining/delimiting character, a **comma** is used by default.

### Default parameters

If you're writing a function and want to support optional parameters, you can specify default values by adding `=` after the name of the parameter, followed by the default value:

{% code lineNumbers="true" %}
```javascript
function hello(name = "Chris") {
  console.log(`Hello ${name}!`);
}

hello("Ari"); // Hello Ari!
hello(); // Hello Chris!
```
{% endcode %}

## Anonymous functions and arrow functions

You can create a function that doesn't have a name:

{% code lineNumbers="true" %}
```javascript
(function () {
  alert("hello");
});
```
{% endcode %}

This is called an **anonymous function**, because it has no name. You'll often see anonymous functions **when a function expects to receive another function as a parameter**. In this case, the function parameter is often passed as an anonymous function.

For example, we can pass the following anonymous function into `addEventListener()`:

{% code lineNumbers="true" %}
```javascript
textBox.addEventListener("keydown", function (event) {
  console.log(`You pressed "${event.key}".`);
});
```
{% endcode %}

{% hint style="info" %}
Here, the idea of [event object](functions.md#event-objects) is used!
{% endhint %}

### Arrow functions

If you pass an anonymous function like this, there's an alternative form you can use, called an **arrow function**. Instead of `function(event)`, you write `(event) =>`:

{% code lineNumbers="true" %}
```javascript
textBox.addEventListener("keydown", (event) => {
  console.log(`You pressed "${event.key}".`);
});
```
{% endcode %}

{% hint style="info" %}
The parentheses around `event` are used for passing several parameters. If the function only takes one parameter, you can omit the parentheses around the parameter.&#x20;
{% endhint %}

We recommend that you use arrow functions, as they can make your code shorter and more readable. To learn more, see the [section on arrow functions in the JavaScript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#arrow_functions), and our [reference page on arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

## Function scope and conflicts

Let's talk a bit about [scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope) — a very important concept when dealing with functions. When you create a function, the variables and other things defined inside the function are inside their own separate **scope**, meaning that they are locked away in their own separate compartments, **unreachable from code outside the functions.**

The top-level outside all your functions is called the **global scope**. Values defined in the global scope are **accessible from everywhere** in the code.

For example, say you have an HTML file that is calling in two external JavaScript files, and both of them have a variable and a function defined that use the same name:

{% code lineNumbers="true" %}
```html
<!-- Excerpt from my HTML -->
<script src="first.js"></script>
<script src="second.js"></script>
<script>
  greeting();
</script>
```
{% endcode %}

{% code title="first.js" lineNumbers="true" %}
```javascript
// first.js
const name = "Chris";
function greeting() {
  alert(`Hello ${name}: welcome to our company.`);
}
```
{% endcode %}

{% code title="second.js" lineNumbers="true" %}
```javascript
// second.js
const name = "Zaptec";
function greeting() {
  alert(`Our company is called ${name}.`);
}
```
{% endcode %}

Both functions you want to call are called `greeting()`, but you can only ever access the `first.js` file's `greeting()` function (the second one is ignored). In addition, an error results when attempting (in the `second.js` file) to assign a new value to the `name` variable — because it was already declared with `const`, and so can't be reassigned.

{% hint style="info" %}
&#x20;You can see this example [running live on GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/functions/conflict.html) provided by MDN(see also the [source code](https://github.com/mdn/learning-area/tree/main/javascript/building-blocks/functions)).
{% endhint %}

## Function return values

As the name suggested, return values are the values that a function returns when it completes. You can have some functions that **have** return values, while others don't have. Both are Okie. However, bear in mind that if you want a function to return a value, use the `return` statement. This is similar to other programming languages also.

