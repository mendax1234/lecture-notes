# JavaScript

## JavaScript Basic

> This part is mainly from [MDN's JavaScript Basics Crash Course](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Your_first_website/Adding_interactivity#language_basics_crash_course). I found it pretty useful and suitable for beginners in JavaScript.

JavaScript is a powerful programming language that can add interactivity to a website. This happens in the behavior of responses when buttons are pressed or with data entry on forms, with dynamic styling, with animation, etc.

### Variables

[Variables](https://developer.mozilla.org/en-US/docs/Glossary/Variable) are containers that store values. Note that variables may hold values that have different [data types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures):

| Variable                                                             | Explanation                                                                                                                      | Example                                                                                                                                                                    |
| -------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [String](https://developer.mozilla.org/en-US/docs/Glossary/String)   | This is a sequence of text known as a string. To signify that the value is a string, enclose it in single or double quote marks. | <p><code>let myVariable = 'Bob';</code> or<br><code>let myVariable = "Bob";</code></p>                                                                                     |
| [Number](https://developer.mozilla.org/en-US/docs/Glossary/Number)   | This is a number. Numbers don't have quotes around them.                                                                         | `let myVariable = 10;`                                                                                                                                                     |
| [Boolean](https://developer.mozilla.org/en-US/docs/Glossary/Boolean) | This is a True/False value. The words `true` and `false` are special keywords that don't need quote marks.                       | `let myVariable = true;`                                                                                                                                                   |
| [Array](https://developer.mozilla.org/en-US/docs/Glossary/Array)     | This is a structure that allows you to store multiple values in a single reference.                                              | <p><code>let myVariable = [1,'Bob','Steve',10];</code><br>Refer to each member of the array like this:<br><code>myVariable[0]</code>, <code>myVariable[1]</code>, etc.</p> |
| [Object](https://developer.mozilla.org/en-US/docs/Glossary/Object)   | This can be anything. **Everything in JavaScript is an object and can be stored in a variable.** Keep this in mind as you learn. | <p><code>let myVariable = document.querySelector('h1');</code><br>All of the above examples too.</p>                                                                       |

After assigning a value to a variable, you can change it later in the code using the `=` operator:

{% code lineNumbers="true" %}
```javascript
let myVariable = "Bob";
myVariable = "Steve";
```
{% endcode %}

### Comments

Comments are snippets of text that can be added along with code. The browser ignores text marked as comments. You can write comments in JavaScript just as you can in C:

{% code lineNumbers="true" %}
```javascript
/*
Everything in between is a comment.
*/
```
{% endcode %}

If your comment contains no line breaks, you can put it behind two slashes like this:

{% code lineNumbers="true" %}
```javascript
// This is a comment
```
{% endcode %}

### Operators

An [`operator`](https://developer.mozilla.org/en-US/docs/Glossary/Operator) is a mathematical symbol that produces a result based on two values (or variables). In the following table, you can see some of the simplest operators, along with some examples to try in the JavaScript console.

| Operator                              | Explanation                                                                                                                                                                                                             | Symbol(s)                                                                                            | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Addition                              | Add two numbers together or combine two strings.                                                                                                                                                                        | `+`                                                                                                  | <p><code>6 + 9;</code><br><code>'Hello ' + 'world!';</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Subtraction, Multiplication, Division | These do what you'd expect them to do in basic math.                                                                                                                                                                    | `-`, `*`, `/`                                                                                        | <p><code>9 - 3;</code><br><code>8 * 2; // multiply in JS is an asterisk</code><br><code>9 / 3;</code></p>                                                                                                                                                                                                                                                                                                                                                                                                           |
| Assignment                            | As you've seen already: this assigns a value to a variable.                                                                                                                                                             | `=`                                                                                                  | `let myVariable = 'Bob';`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Strict equality                       | This performs a test to see if two values are equal and of the same data type. It returns a `true`/`false` (Boolean) result.                                                                                            | [`===`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) | <p><code>let myVariable = 3;</code><br><code>myVariable === 4;</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Not, Does-not-equal                   | This returns the logically opposite value of what it precedes. It turns a `true` into a `false`, etc.. When it is used alongside the Equality operator, the negation operator tests whether two values are _not_ equal. | `!`, `!==`                                                                                           | <p>For "Not", the basic expression is <code>true</code>, but the comparison returns <code>false</code> because we negate it:</p><p><code>let myVariable = 3;</code><br><code>!(myVariable === 3);</code></p><p>"Does-not-equal" gives basically the same result with different syntax. Here we are testing "is <code>myVariable</code> NOT equal to 3". This returns <code>false</code> because <code>myVariable</code> IS equal to 3:</p><p><code>let myVariable = 3;</code><br><code>myVariable !== 3;</code></p> |

{% hint style="info" %}
**Note:** Mixing data types can lead to some strange results when performing calculations. Be careful that you are referring to your variables correctly, and getting the results you expect. For example, enter `'35' + '25'` into your console. Why don't you get the result you expected? Because the quote marks turn the numbers into strings, so you've ended up concatenating strings rather than adding numbers. If you enter `35 + 25` you'll get the total of the two numbers.
{% endhint %}

### Conditionals

Conditionals are code structures used to test if an expression returns true or not. A very common form of conditionals is the `if...else` statement. For example:

{% code lineNumbers="true" %}
```javascript
let iceCream = "chocolate";
if (iceCream === "chocolate") {
  alert("Yay, I love chocolate ice cream!");
} else {
  alert("Awwww, but chocolate is my favorite…");
}
```
{% endcode %}

The expression inside the `if ()` is the test. This uses the strict equality operator (as described above) to compare the variable `iceCream` with the string `chocolate` to see if the two are equal. If this comparison returns `true`, the first block of code runs. If the comparison is not true, the second block of code—after the `else` keyword—runs instead.

### Functions

[Functions](https://developer.mozilla.org/en-US/docs/Glossary/Function) are a way of packaging functionality that you wish to reuse. It's possible to define a body of code as a function that executes when you call the function name in your code. This is a good alternative to repeatedly writing the same code. You have already seen some uses of functions. For example:

{% code lineNumbers="true" %}
```javascript
let myVariable = document.querySelector("h1");
```
{% endcode %}

{% code lineNumbers="true" %}
```javascript
alert("hello");
```
{% endcode %}

The `document.querySelector()` and `alert()` functions are built into the browser.

If you see something which looks like a variable name, but it's followed by parentheses — `()` — it is likely to be a function. Functions often take [arguments](https://developer.mozilla.org/en-US/docs/Glossary/Argument): bits of data they need to do their job. Arguments go inside the parentheses, separated by commas if there is more than one argument.

For example, the `alert()` function makes a pop-up box appear inside the browser window, but we need to give it a string as an argument to tell the function what message to display.

You can also define your own functions. In the next example, we create a simple function which takes two numbers as arguments and multiplies them:

{% code lineNumbers="true" %}
```javascript
function multiply(num1, num2) {
  let result = num1 * num2;
  return result;
}
```
{% endcode %}

{% hint style="info" %}
**Note:** The [`return`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/return) statement tells the browser to return the `result` variable out of the function so it is available to use. This is necessary because variables defined inside functions are only available inside those functions. This is called variable [scoping](https://developer.mozilla.org/en-US/docs/Glossary/Scope). (Read more about [variable scoping](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variable_scope).)
{% endhint %}

### Events

Real interactivity on a website requires event handlers. These are code structures that listen for activity in the browser, and run code in response. The most obvious example is handling the [click event](https://developer.mozilla.org/en-US/docs/Web/API/Element/click_event), which is fired by the browser when you click on something with your mouse. To demonstrate this, enter the following into your console, then click on the current webpage:

{% code lineNumbers="true" %}
```javascript
document.querySelector("html").addEventListener("click", function () {
  alert("Ouch! Stop poking me!");
});
```
{% endcode %}

There are a number of ways to attach an event handler to an element. Here we select the [`<html>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/html) element. We then call its [`addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) function, passing in the name of the event to listen for (`'click'`) and a function to run when the event happens.

The function we just passed to `addEventListener()` here is called an _anonymous function_, because it doesn't have a name. There's an alternative way of writing anonymous functions, which we call an _arrow function_. An arrow function uses `() =>` instead of `function ()`:

{% code lineNumbers="true" %}
```javascript
document.querySelector("html").addEventListener("click", () => {
  alert("Ouch! Stop poking me!");
});
```
{% endcode %}

## Learning Resources

Here are some useful resources I found when learning JavaScript Basics

* [JavaScript at the Mozilla Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
* [Learning JavaScript](https://learnjavascript.online/) (The first 40 lessons are free, but the remaining are paid)
