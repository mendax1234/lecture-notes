# Strings

## Declaring strings

This is similar to declaring a [number](numbers-and-operators.md#numbers), except that strings must be surrounded with quotes.

### Single quotes, double quotes and backticks

In JavaScript, you can choose single quotes (`'`), double quotes (`"`), or backticks (`` ` ``) to wrap your strings in. All of the following will work:

{% code lineNumbers="true" %}
```javascript
const single = 'Single quotes';
const double = "Double quotes";
const backtick = `Backtick`;

console.log(single);
console.log(double);
console.log(backtick);
```
{% endcode %}

{% hint style="info" %}
You must use the same character for the start and end of a string, or you will get an error.
{% endhint %}

Strings declared using single quotes and strings declared using double quotes are the same, and which you use is down to personal preference — although it is good practice to choose one style and use it consistently in your code.

Strings declared using backticks are a special kind of string called a [_template literal_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals). In most ways, template literals are like normal strings, but they have some special properties:

* you can [embed JavaScript](strings.md#embedding-javascript) in them
* you can declare template literals over [multiple lines](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Strings#multiline_strings)

## Embedding JavaScript

Inside a template literal, you can wrap **JavaScript variables or expressions** inside `${ }`, and the result will be included in the string:

{% code lineNumbers="true" %}
```javascript
const name = "Chris";
const greeting = `Hello, ${name}`;
console.log(greeting); // "Hello, Chris"
```
{% endcode %}

You can use the same technique to join together two variables:

{% code lineNumbers="true" %}
```javascript
const one = "Hello, ";
const two = "how are you?";
const joined = `${one}${two}`;
console.log(joined); // "Hello, how are you?"
```
{% endcode %}

Joining strings together like this is called _concatenation_.

### Concatenation using `+`

You can use `${}` **only with template literals**, not normal strings. You can concatenate normal strings using the `+` operator:

{% code lineNumbers="true" %}
```javascript
const greeting = "Hello";
const name = "Chris";
console.log(greeting + ", " + name); // "Hello, Chris"
```
{% endcode %}

However, template literals usually give you more readable code:

{% code lineNumbers="true" %}
```javascript
const greeting = "Hello";
const name = "Chris";
console.log(`${greeting}, ${name}`); // "Hello, Chris"
```
{% endcode %}

### Including expressions in strings

You can include JavaScript expressions in template literals, as well as just variables, and the results will be included in the result:

{% code overflow="wrap" lineNumbers="true" %}
```javascript
const song = "Fight the Youth";
const score = 9;
const highestScore = 10;
const output = `I like the song ${song}. I gave it a score of ${
  (score / highestScore) * 100
}%.`;
console.log(output); // "I like the song Fight the Youth. I gave it a score of 90%."
```
{% endcode %}

## Multiline strings

Template literals respect the line breaks in the source code, so you can write strings that span multiple lines like this:

{% code lineNumbers="true" %}
```javascript
const newline = `One day you finally knew
what you had to do, and began,`;
console.log(newline);

/*
One day you finally knew
what you had to do, and began,
*/
```
{% endcode %}

See MDN's [Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) reference page for more examples and details of advanced features.

## Including quotes in strings

It is recommended to use the escapeletter `\` to achieve this.

{% code lineNumbers="true" %}
```javascript
const bigmouth = 'I\'ve got no right to take my place…';
console.log(bigmouth); // I've got no right to take my place…
```
{% endcode %}

{% hint style="info" %}
You can use the escapeletter `\` technique to insert other special characters. See [Escape sequences](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#escape_sequences) for more details.
{% endhint %}

## Numbers vs. strings

What happens when we try to concatenate a string and a number? Let's try it in our console:

{% code lineNumbers="true" %}
```javascript
const name = "Front ";
const number = 242;
console.log(name + number); // "Front 242"
```
{% endcode %}

You might expect this to return an error, but it works just fine. How numbers should be displayed as strings is fairly well-defined, so the browser automatically converts the number to a string and concatenates the two strings.

If you have a numeric variable that you want to convert to a string or a string variable that you want to convert to a number, you can use the following two constructs:

*   The [`Number()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/Number) function converts anything passed to it into a number if it can. Try the following:

    <pre class="language-javascript" data-line-numbers><code class="lang-javascript">const myString = "123";
    const myNum = Number(myString);
    console.log(typeof myNum);
    // number
    </code></pre>
*   Conversely, the [`String()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/String) function converts its argument to a string. Try this:

    <pre class="language-javascript" data-line-numbers><code class="lang-javascript">const myNum2 = 123;
    const myString2 = String(myNum2);
    console.log(typeof myString2);
    // string
    </code></pre>

These constructs can be really useful in some situations. For example, if a user enters a number into a form's text field, it's a string. However, if you want to add this number to something, you'll need it to be a number, so you could pass it through `Number()` to handle this. We did exactly this in our [Number Guessing Game](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game.html), in the `checkGuess` function.

## Useful string methods

### Strings as objects

Most values can be used as if they are objects in JavaScript. When you create a string, for example by using

{% code lineNumbers="true" %}
```javascript
const string = "This is my string";
```
{% endcode %}

although the variable itself [isn't an object](#user-content-fn-1)[^1], it still has a large number of properties and methods available to it, by virtue of being usable as an object when accessing properties.

{% hint style="info" %}
For the full list of all the **static methods** and **instance methods** of the `String` object, you can find it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String).
{% endhint %}



[^1]: Actually, it is an instance of the `String` class
