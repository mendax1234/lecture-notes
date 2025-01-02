# Events

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

## Using `addEventListener()`

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

### Removing listeners

If you've added an event handler using `addEventListener()`, you can remove it again using the [`removeEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener) method. For example, this would remove the `changeBackground()` event handler:

{% code lineNumbers="true" %}
```javascript
btn.removeEventListener("click", changeBackground);
```
{% endcode %}

### Adding multiple listeners for a single event

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

## Event objects

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

### Extra properties of event objects

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

## Preventing default behavior

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

## It's not just web pages

Events are **not unique** to JavaScript — most programming languages have some kind of event model, and the way the model works often differs from JavaScript's way. In fact, the event model in JavaScript for web pages differs from the event model for JavaScript as it is used in other environments.

For example, [Node.js](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) is a very popular JavaScript runtime that enables developers to use JavaScript to build network and server-side applications. The [Node.js event model](https://nodejs.org/api/events.html) relies on listeners to listen for events and emitters to emit events periodically — it doesn't sound that different, but the code is quite different, making use of functions like `on()` to register an event listener, and `once()` to register an event listener that unregisters after it has run once. The [HTTP connect event docs](https://nodejs.org/api/http.html#event-connect) provide a good example.

You can also use JavaScript to build cross-browser add-ons — browser functionality enhancements — using a technology called [WebExtensions](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions). The event model is similar to the web events model, but a bit different — event listeners' properties are written in [camel case](https://developer.mozilla.org/en-US/docs/Glossary/Camel_case) (such as `onMessage` rather than `onmessage`), and need to be combined with the `addListener` function. See the [`runtime.onMessage`](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onMessage#examples) page for an example.

You don't need to understand anything about other such environments at this stage in your learning; we just wanted to make it clear that events can differ in different programming environments.
