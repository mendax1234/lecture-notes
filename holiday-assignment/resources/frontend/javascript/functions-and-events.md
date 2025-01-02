# Functions and events

## Functions

One essential concept in coding is **functions**, which allow you to store a piece of code that does a single task inside a defined block, and then call that code whenever you need it using a single short command — rather than having to type out the same code multiple times.

### Functions vs. methods

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

### Function parameters

Some functions require **parameters** to be specified when you are invoking them — these are values that need to be included inside the function parentheses, which it needs to do its job properly.

#### Optional parameters

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

#### Default parameters

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

### Anonymous functions and arrow functions

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
Here, the idea of [event object](functions-and-events.md#event-objects) is used!
{% endhint %}

#### Arrow functions

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

### Function scope and conflicts

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

### Function return values

As the name suggested, return values are the values that a function returns when it completes. You can have some functions that **have** return values, while others don't have. Both are Okie. However, bear in mind that if you want a function to return a value, use the `return` statement. This is similar to other programming languages also.

## Events

Events are things that happen in the system you are programming, which the system tells you about so your code can react to them. For example, if the user clicks a button on a webpage, you might want to react to that action by displaying an information box.

{% hint style="info" %}
You can gather from glancing at the MDN [event reference](https://developer.mozilla.org/en-US/docs/Web/Events) that there are **a lot** of events that can be fired.
{% endhint %}

To react to an event, you attach an **event handler** to it. This is a block of code (usually a JavaScript function that you as a programmer create) that runs when the event fires. When such a block of code is defined to run in response to an event, we say we are **registering an event handler**.

{% hint style="info" %}
Event handlers are sometimes called **event listeners** — they are pretty much interchangeable for our purposes, although strictly speaking, they work together. The listener listens out for the event happening, and the handler is the code that runs in response to it happening.
{% endhint %}

For example, we want to handle a `click` event. Firstly, we have a `<button>` in the page,

{% code lineNumbers="true" %}
```html
<button>Change color</button>
```
{% endcode %}

Then we have some JavaScript We'll look at this in more detail in the next section, but for now we can just say: it adds an event handler to the button's `"click"` event, and the handler reacts to the event by setting the page background to a random color:

{% code lineNumbers="true" %}
```javascript
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.addEventListener("click", () => {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
});
```
{% endcode %}

### Using `addEventListener()`

As we saw in the last example, objects that can fire events have an [`addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) method, and this is the recommended mechanism for adding event handlers.

Let's take a closer look at the code from the last example:

{% code lineNumbers="true" %}
```javascript
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.addEventListener("click", () => {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
});
```
{% endcode %}

The HTML [`<button>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) element will fire an event when the user clicks the button. So it defines an `addEventListener()` function, which we are calling here. We're passing in two parameters:

* the string `"click"`, to indicate that we want to listen to the click event. Buttons can fire lots of other events, such as [`"mouseover"`](https://developer.mozilla.org/en-US/docs/Web/API/Element/mouseover_event) when the user moves their mouse over the button, or [`"keydown"`](https://developer.mozilla.org/en-US/docs/Web/API/Element/keydown_event) when the user presses a key and the button is focused.
* a function to call when the event happens. In our case, the function generates a random RGB color and sets the [`background-color`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-color) of the page [`<body>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/body) to that color.

#### Removing listeners

If you've added an event handler using `addEventListener()`, you can remove it again using the [`removeEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener) method. For example, this would remove the `changeBackground()` event handler:

{% code lineNumbers="true" %}
```javascript
btn.removeEventListener("click", changeBackground);
```
{% endcode %}

#### Adding multiple listeners for a single event

By making more than one call to [`addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener), providing different handlers, you can have multiple handlers for a single event:

{% code lineNumbers="true" %}
```javascript
myElement.addEventListener("click", functionA);
myElement.addEventListener("click", functionB);
```
{% endcode %}

Both functions would now run when the element is clicked.

{% hint style="info" %}
Other there are other event listening mechanism, We recommend that you use `addEventListener()` to register event handlers. It's the most powerful method and scales best with more complex programs. But out of interest, if you want to know more, you can learn the other two methods from [here](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events#other_event_listener_mechanisms).
{% endhint %}

### Event objects

Sometimes, inside an event handler function, you'll see a parameter specified with a name such as `event`, `evt`, or `e`. This is called the **event object**, and it is automatically passed to event handlers to provide extra features and information. For example, let's rewrite our random color example again slightly:

{% code lineNumbers="true" %}
```javascript
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

function bgChange(e) {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  e.target.style.backgroundColor = rndCol;
  console.log(e);
}

btn.addEventListener("click", bgChange);
```
{% endcode %}

Here you can see we are including an event object, **e**, in the function, and in the function setting a background color style on `e.target` — which is the button itself (The original one is `document.body`). The `target` property of the event object is always a reference to the element the event occurred upon. So, in this example, we are setting a random background color on the button, not the page.

{% hint style="info" %}
You can use any name you like for the event object — you just need to choose a name that you can then use to reference it inside the event handler function. `e`/`evt`/`event` is most commonly used by developers because they are short and easy to remember. It's always good to be consistent — with yourself, and with others if possible.
{% endhint %}

#### Extra properties of event objects

Most event objects have a standard set of properties and methods available on the event object; see the [`Event`](https://developer.mozilla.org/en-US/docs/Web/API/Event) object reference from MDN for a full list.

Some event objects add extra properties that are relevant to that particular type of event. For example, the [`keydown`](https://developer.mozilla.org/en-US/docs/Web/API/Element/keydown_event) event fires when the user presses a key. Its event object is a [`KeyboardEvent`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent), which is a specialized `Event` object with a `key` property that tells you which key was pressed:

{% code lineNumbers="true" %}
```html
<input id="textBox" type="text" />
<div id="output"></div>
```
{% endcode %}

{% code lineNumbers="true" %}
```javascript
const textBox = document.querySelector("#textBox");
const output = document.querySelector("#output");
textBox.addEventListener("keydown", (event) => {
  output.textContent = `You pressed "${event.key}".`;
});
```
{% endcode %}

### Preventing default behavior

Sometimes, you'll come across a situation where you want to prevent an event from doing what it does by default. The most common example is that of a web form, for example, a custom registration form. When you fill in the details and click the submit button, the natural behavior is for the data to be submitted to a specified page on the server for processing, and the browser to be redirected to a "success message" page of some kind (or the same page, if another is not specified).

The trouble comes when the user has not submitted the data **correctly** — as a developer, you want to prevent the submission to the server and give an error message saying what's wrong and what needs to be done to put things right. Some browsers support automatic form data validation features, but since many don't, you are advised to not rely on those and implement your own validation checks. Let's look at an example.

First, a simple HTML form that requires you to enter your first and last name:

{% code lineNumbers="true" %}
```html
<form>
  <div>
    <label for="fname">First name: </label>
    <input id="fname" type="text" />
  </div>
  <div>
    <label for="lname">Last name: </label>
    <input id="lname" type="text" />
  </div>
  <div>
    <input id="submit" type="submit" />
  </div>
</form>
<p></p>
```
{% endcode %}

Now some JavaScript — here we implement a very simple check inside a handler for the [`submit`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event) event (the submit event is fired on a form when it is submitted) that tests whether the text fields are empty. If they are, we call the [`preventDefault()`](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault) function on the event object — which **stops the form submission** — and then display an error message in the paragraph below our form to tell the user what's wrong:

{% code lineNumbers="true" %}
```javascript
const form = document.querySelector("form");
const fname = document.getElementById("fname");
const lname = document.getElementById("lname");
const para = document.querySelector("p");

form.addEventListener("submit", (e) => {
  if (fname.value === "" || lname.value === "") {
    e.preventDefault();
    para.textContent = "You need to fill in both names!";
  }
});
```
{% endcode %}

Obviously, this is pretty weak form validation — it wouldn't stop the user from validating the form with spaces or numbers entered into the fields, for example — but it is OK for example purposes.

### It's not just web pages

Events are **not unique** to JavaScript — most programming languages have some kind of event model, and the way the model works often differs from JavaScript's way. In fact, the event model in JavaScript for web pages differs from the event model for JavaScript as it is used in other environments.

For example, [Node.js](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) is a very popular JavaScript runtime that enables developers to use JavaScript to build network and server-side applications. The [Node.js event model](https://nodejs.org/api/events.html) relies on listeners to listen for events and emitters to emit events periodically — it doesn't sound that different, but the code is quite different, making use of functions like `on()` to register an event listener, and `once()` to register an event listener that unregisters after it has run once. The [HTTP connect event docs](https://nodejs.org/api/http.html#event-connect) provide a good example.

You can also use JavaScript to build cross-browser add-ons — browser functionality enhancements — using a technology called [WebExtensions](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions). The event model is similar to the web events model, but a bit different — event listeners' properties are written in [camel case](https://developer.mozilla.org/en-US/docs/Glossary/Camel_case) (such as `onMessage` rather than `onmessage`), and need to be combined with the `addListener` function. See the [`runtime.onMessage`](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onMessage#examples) page for an example.

You don't need to understand anything about other such environments at this stage in your learning; we just wanted to make it clear that events can differ in different programming environments.
