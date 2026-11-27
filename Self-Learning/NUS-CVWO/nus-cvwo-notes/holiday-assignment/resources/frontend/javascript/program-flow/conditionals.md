# Conditionals

## `if...else` statements

Let's look at by far the most common type of conditional statement you'll use in JavaScript — the humble [`if...else` statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else).

### Basic `if...else` syntax

Basic `if...else` syntax looks like this:

{% code lineNumbers="true" %}
```javascript
if (condition) {
  /* code to run if condition is true */
} else {
  /* run some other code instead */
}
```
{% endcode %}

This code is pretty human-readable — it is saying "**if** the **condition** returns `true`, run code A, **else** run code B".

### `else if`

> What if we want more than two choices?

There is a way to chain on extra choices/outcomes to your `if...else` — using `else if`. Each extra choice requires an additional block to put in between `if () { }` and `else { }` — check out the following more involved example, which could be part of a simple weather forecast application:

{% code lineNumbers="true" %}
```html
<label for="weather">Select the weather type today: </label>
<select id="weather">
  <option value="">--Make a choice--</option>
  <option value="sunny">Sunny</option>
  <option value="rainy">Rainy</option>
  <option value="snowing">Snowing</option>
  <option value="overcast">Overcast</option>
</select>

<p></p>
```
{% endcode %}

{% code overflow="wrap" lineNumbers="true" %}
```javascript
const select = document.querySelector("select");
const para = document.querySelector("p");

select.addEventListener("change", setWeather);

function setWeather() {
  const choice = select.value;

  if (choice === "sunny") {
    para.textContent =
      "It is nice and sunny outside today. Wear shorts! Go to the beach, or the park, and get an ice cream.";
  } else if (choice === "rainy") {
    para.textContent =
      "Rain is falling outside; take a rain coat and an umbrella, and don't stay out for too long.";
  } else if (choice === "snowing") {
    para.textContent =
      "The snow is coming down — it is freezing! Best to stay in with a cup of hot chocolate, or go build a snowman.";
  } else if (choice === "overcast") {
    para.textContent =
      "It isn't raining, but the sky is grey and gloomy; it could turn any minute, so take a rain coat just in case.";
  } else {
    para.textContent = "";
  }
}
```
{% endcode %}

1. Here we've got an HTML [`<select>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select) element allowing us to make different weather choices, and a simple paragraph.
2. In the JavaScript, we are storing a reference to both the [`<select>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select) and [`<p>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p) elements, and adding an event listener to the `<select>` element so that when its value is changed, the `setWeather()` function is run.
3. When this function is run, we first set a variable called `choice` to the current value selected in the `<select>` element. We then use a conditional statement to show different text inside the paragraph depending on what the value of `choice` is. Notice how all the conditions are tested in `else if () { }` blocks, except for the first one, which is tested in an `if () { }` block.
4. The very last choice, inside the `else { }` block, is basically a "last resort" option — the code inside it will be run if none of the conditions are `true`. In this case, it serves to empty the text out of the paragraph if nothing is selected, for example, if a user decides to re-select the "--Make a choice--" placeholder option shown at the beginning.

{% hint style="info" %}
In JavaScript, any value that is not `false`, `undefined`, `null`, `0`, `NaN`, or an empty string (`''`) actually returns `true` when tested as a conditional statement.
{% endhint %}

## `switch` statements

`if...else` statements do the job of enabling conditional code well, but they are not without their downsides. They are mainly good for cases where you've got a couple of choices, and each one requires a reasonable amount of code to be run, and/or the conditions are complex (for example, multiple logical operators). For cases where you just want to set a variable to a certain choice of value or print out a particular statement depending on a condition, the syntax can be a bit cumbersome, especially if you've got a large number of choices.

In such a case, [`switch` statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) are your friend — they take a **single expression/value** as an input, and then look through several choices until they find one that **matches that value**, executing the corresponding code that goes along with it. Here's some more pseudocode, to give you an idea:

{% code lineNumbers="true" %}
```javascript
switch (expression) {
  case choice1:
    // run this code
    break;

  case choice2:
    // run this code instead
    break;

  // include as many cases as you like

  default:
    // actually, just run this code
    break;
}
```
{% endcode %}

Here we've got:

1. The keyword `switch`, followed by a set of parentheses.
2. An expression or value inside the parentheses.
3. The keyword `case`, followed by a choice that the expression/value could be, followed by a colon.
4. Some code to run if the choice matches the expression.
5. A `break` statement, followed by a semicolon. If the previous choice matches the expression/value, the browser stops executing the code block here, and moves on to any code that appears below the switch statement.
6. As many other cases (bullets 3–5) as you like.
7. The keyword `default`, followed by exactly the same code pattern as one of the cases (bullets 3–5), except that `default` does not have a choice after it, and you don't need the `break` statement as there is nothing to run after this in the block anyway. This is the default option that runs if none of the choices match.

{% hint style="info" %}
You don't have to include the `default` section — you can safely omit it if there is no chance that the expression could end up equaling an unknown value. If there is a chance of this, however, you need to include it to handle unknown cases.
{% endhint %}

## Tenary operator

There is one final bit of syntax we want to introduce you to before we get you to play with some examples. The [ternary or conditional operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator) is a small bit of syntax that tests a condition and returns one value/expression if it is `true`, and another if it is `false` — this can be useful in some situations, and can take up a lot less code than an `if...else` block if you have **two** choices that are chosen between via a `true`/`false` condition. The pseudocode looks like this:

{% code lineNumbers="true" %}
```
condition ? run this code : run this code instead
```
{% endcode %}

So let's look at an example:

{% code lineNumbers="true" %}
```javascript
const greeting = isBirthday
  ? "Happy birthday Mrs. Smith — we hope you have a great day!"
  : "Good morning Mrs. Smith.";
```
{% endcode %}
