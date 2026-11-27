# Client-side framework

## Libraries and frameworks

When JavaScript debuted in 1996, it added occasional interactivity and excitement to a web that was, up until then, composed of static documents. The web became not just a place to _read things_, but to _do things_. JavaScript's popularity steadily increased. _Developers who worked with JavaScript wrote tools to solve the problems they faced, and packaged them into reusable packages_ called **libraries**, so they could share their solutions with others. This shared ecosystem of libraries helped shape the growth of the web, and eventually gave way to frameworks.

A **framework** is _a library that offers opinions about how software gets built_. These opinions allow for predictability and homogeneity in an application; predictability allows the software to scale to an enormous size and still be maintainable; predictability and maintainability are essential for the health and longevity of software. The advent of modern JavaScript frameworks has made it much easier to build highly dynamic, interactive applications.

JavaScript frameworks power much of the impressive software on the modern web – including many of the websites you likely use every day.

One of the uses of JavaScript frameworks is that it offers a way to **write user interfaces more&#x20;**_**declaratively**_. That is, they allow you to write code that describes how your UI should look, and the framework makes it happen in the DOM behind the scenes.

## Current frameworks

There are many frameworks out there, but currently the "big four" are considered to be the following.

1. [Ember](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction#ember)
2. [Angular](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction#angular)
3. [Vue](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction#vue)
4.  [React](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction#react)\
    Facebook released [React](https://react.dev/) in 2013. By this point, it had already been using React to solve many of its problems internally. Technically, React itself is _not_ a framework; it's a library for rendering UI components. React is used in combination with _other_ libraries to make applications — React and [React Native](https://reactnative.dev/) enable developers to make mobile applications; React and [ReactDOM](https://react.dev/reference/react-dom) enable them to make web applications, etc.

    Because React and ReactDOM are so often used together, React is colloquially understood as a JavaScript framework. As you read through this module, we will be working with that colloquial understanding.

The table in this section provides a glanceable summary of the current _browser support_ offered by each framework, as well as the **domain-specific languages** with which it can be used.

Broadly, [domain-specific languages (DSLs)](https://developer.mozilla.org/en-US/docs/Glossary/DSL/Domain_specific_language) are programming languages relevant in specific areas of software development. In the context of frameworks, DSLs are variations on JavaScript or HTML that make it easier to develop with that framework. Crucially, none of the frameworks _require_ a developer to use a specific DSL, but they have almost all been designed with a specific DSL in mind. Choosing not to employ a framework's preferred DSL will mean you miss out on features that would otherwise improve your developer experience.

You should seriously consider the support matrix and DSLs of a framework when making a choice for any new project. Mismatched browser support can be a barrier to your users; mismatched DSL support can be a barrier to you and your teammates.

| Framework | Browser support                     | Preferred DSL | Supported DSLs         | Citation                                                                        |
| --------- | ----------------------------------- | ------------- | ---------------------- | ------------------------------------------------------------------------------- |
| Angular   | Modern                              | TypeScript    | HTML-based; TypeScript | [official docs](https://angular.dev/guide/browser-support)                      |
| React     | Modern                              | JSX           | JSX; TypeScript        | [official docs](https://react.dev/reference/react-dom/client#browser-support)   |
| Vue       | Modern (IE9+ in Vue 2)              | HTML-based    | HTML-based, JSX, Pug   | [official docs](https://cli.vuejs.org/guide/browser-compatibility.html)         |
| Ember     | Modern (IE9+ in Ember version 2.18) | Handlebars    | Handlebars, TypeScript | [official docs](https://guides.emberjs.com/v3.3.0/templates/handlebars-basics/) |

{% hint style="info" %}
But for CVWO, we have no choice but to choose React as our JS framework.
{% endhint %}

## Domain-specific languages

### JSX

[JSX](https://react.dev/learn/writing-markup-with-jsx), which stands for JavaScript and XML, is an extension of JavaScript that brings HTML-like syntax to a JavaScript environment. It was invented by the React team for use in React applications, but can be used to develop other applications — like Vue apps, for instance.

The following shows a simple JSX example:

{% code lineNumbers="true" %}
```jsx
const subject = "World";
const header = (
  <header>
    <h1>Hello, {subject}!</h1>
  </header>
);
```
{% endcode %}

This expression represents an HTML [`<header>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/header) element with an [`<h1>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) element inside. The curly braces around `{subject}` tell the application to read the value of the `subject` constant and insert it into our `<h1>`.

When used with React, the JSX from the previous snippet would be compiled into this:

{% code lineNumbers="true" %}
```javascript
const subject = "World";
const header = React.createElement(
  "header",
  null,
  React.createElement("h1", null, "Hello, ", subject, "!"),
);
```
{% endcode %}

When ultimately rendered by the browser, the above snippet will produce HTML that looks like this:

```html
<header>
  <h1>Hello, World!</h1>
</header>
```

### TypeScript

[TypeScript](https://www.typescriptlang.org/) is a _superset_ of JavaScript, meaning it extends JavaScript — all JavaScript code is valid TypeScript, but not the other way around. TypeScript is useful for the strictness it allows developers to enforce on their code. For instance, consider a function `add()`, which takes integers `a` and `b` and returns their sum.

In JavaScript, that function could be written like this:

{% code lineNumbers="true" %}
```javascript
function add(a, b) {
  return a + b;
}
```
{% endcode %}

This code might be trivial for someone accustomed to JavaScript, but it could still be clearer. JavaScript lets us use the `+` operator to concatenate strings together, so this function would technically still work if `a` and `b` were strings — it just **might not** give you the result you'd expect. What if we wanted to only allow numbers to be passed into this function? TypeScript makes that possible:

```typescript
function add(a: number, b: number) {
  return a + b;
}
```

The `: number` written after each parameter here tells TypeScript that both `a` and `b` must be numbers. If we were to use this function and pass `'2'` into it as an argument, TypeScript would raise an error during compilation, and we would be forced to fix our mistake. We could write our own JavaScript that raises these errors for us, but it would make our source code significantly more verbose. It probably makes more sense to let TypeScript handle such checks for us.

{% hint style="info" %}
For more information on TypeSciprt, please see [here](../../typescript.md).
{% endhint %}
