# JS OOP features

## Object basics

An object is a collection of related data and/or functionality. These usually consist of several variables and functions (which are called _properties_ and _methods_ when they are **inside objects**).

Firstly, an object is made up of multiple members, each of which has a name

{% code lineNumbers="true" %}
```javascript
const objectName = {
  member1Name: member1Value,
  member2Name: member2Value,
  member3Name: member3Value,
};
```
{% endcode %}

The value of an object member can be pretty much anything, like a number, an array, and a function. For example, here is our `person` object:

{% code lineNumbers="true" %}
```javascript
const person = {
  name: ["Bob", "Smith"],
  age: 32,
  bio: function () {
    console.log(`${this.name[0]} ${this.name[1]} is ${this.age} years old.`);
  },
  introduceSelf: function () {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```
{% endcode %}

In our `person` object, the first two items are data items, and are referred to as the object's **properties**. The last two items are functions that allow the object to do something with that data, and are referred to as the object's **methods**.

When the object's members are functions there's a simpler syntax. Instead of `bio: function ()` we can write `bio()`. Like this:

{% code lineNumbers="true" %}
```javascript
const person = {
  name: ["Bob", "Smith"],
  age: 32,
  bio() {
    console.log(`${this.name[0]} ${this.name[1]} is ${this.age} years old.`);
  },
  introduceSelf() {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```
{% endcode %}

After creating this object, you can access its members using the code as follows:

{% code lineNumbers="true" %}
```javascript
person.name;
person.name[0];
person.age;
person.bio();
// "Bob Smith is 32 years old."
person.introduceSelf();
// "Hi! I'm Bob."
```
{% endcode %}

An object like this is referred to as an **object literal** — we've literally written out the object contents as we've come to create it. This is different compared to objects instantiated from classes, which we'll look at later on.

It is very common to create an object using an object literal when you want to transfer a series of structured, related data items in some manner, for example sending a request to the server to be put into a database. Sending a **single object** is much more efficient than sending several items individually, and it is easier to work with than an array, when you want to identify individual items by name.

## Dot notation

Above, you accessed the object's properties and methods using **dot notation**. The object name (person) acts as the **namespace** — it must be entered first to access anything inside the object. Next you write a dot, then the item you want to access — this can be the name of a simple property, an item of an array property, or a call to one of the object's methods, for example:

{% code lineNumbers="true" %}
```javascript
person.age;
person.bio();
```
{% endcode %}

### Objects as object properties

An object property can itself be an object. For example, try changing the `name` member from

{% code lineNumbers="true" %}
```javascript
const person = {
  name: ["Bob", "Smith"],
};
```
{% endcode %}

to

{% code lineNumbers="true" %}
```javascript
const person = {
  name: {
    first: "Bob",
    last: "Smith",
  },
  // …
};
```
{% endcode %}

To access these items you just need to chain the extra step onto the end with another dot.

{% code lineNumbers="true" %}
```javascript
person.name.first;
person.name.last;
```
{% endcode %}

If you do this, you'll also need to go through your method code and change any instances of

{% code lineNumbers="true" %}
```javascript
name[0];
name[1];
```
{% endcode %}

to

{% code lineNumbers="true" %}
```javascript
name.first;
name.last;
```
{% endcode %}

Otherwise, your methods will no longer work.

## Bracket Notation

Bracket notation provides an alternative way to access object properties. Instead of using [dot notation](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Object_basics#dot_notation) like this:

{% code lineNumbers="true" %}
```javascript
person.age;
person.name.first;
```
{% endcode %}

You can instead use square brackets:

{% code lineNumbers="true" %}
```javascript
person["age"];
person["name"]["first"];
```
{% endcode %}

This looks very similar to how you access the items in an array, and it is basically the same thing — instead of using an index number to select an item, you are using the **name associated with each member's value**. It is no wonder that objects are sometimes called **associative arrays** — they map **strings to values** in the same way that arrays map **numbers to values**.

**Dot notation is generally preferred over bracket notation** because it is more succinct and easier to read. However there are some cases where you have to use square brackets. For example, if an object property name is held in a variable, then you can't use dot notation to access the value, but you can access the value using bracket notation.

## Update object members

So far we've only looked at retrieving (or **getting**) object members — you can also **update** (set) the value of object members by declaring the member you want to set (using dot or bracket notation), like this:

{% code lineNumbers="true" %}
```javascript
person.age = 45;
person["name"]["last"] = "Cratchit";
```
{% endcode %}

Setting members doesn't just stop at updating the values of existing properties and methods; you can also **create completely new members**.

{% code lineNumbers="true" %}
```javascript
person["eyes"] = "hazel";
person.farewell = function () {
  console.log("Bye everybody!");
};
```
{% endcode %}

You can now test out your new members:

{% code lineNumbers="true" %}
```javascript
person["eyes"];
person.farewell();
// "Bye everybody!"
```
{% endcode %}

## What is "this"?

You may have noticed something slightly strange in our methods. Look at this one for example:

{% code lineNumbers="true" %}
```javascript
introduceSelf() {
  console.log(`Hi! I'm ${this.name[0]}.`);
}
```
{% endcode %}

You are probably wondering what "this" is. The `this` keyword typically refers to **the current object the code is being executed in**. In the context of an object method, `this` refers to the object that the method was called on.

Let's illustrate what we mean with a simplified pair of person objects:

{% code lineNumbers="true" %}
```javascript
const person1 = {
  name: "Chris",
  introduceSelf() {
    console.log(`Hi! I'm ${this.name}.`);
  },
};

const person2 = {
  name: "Deepti",
  introduceSelf() {
    console.log(`Hi! I'm ${this.name}.`);
  },
};
```
{% endcode %}

In this case, `person1.introduceSelf()` outputs "Hi! I'm Chris."; `person2.introduceSelf()` outputs "Hi! I'm Deepti." This happens because when the method is called, `this` refers to the object on which the method is called, which allows the same method definition to work for multiple objects.

This isn't hugely useful when you are writing out object literals by hand, as using the object's name (`person1` and `person2`) leads to the exact same result, but it will be essential when we start using **constructors** to create more than one object from a single object definition, and that's the subject of the next section.

## Introducing constructors

Using object literals is fine when you only need to create **one object**, but if you have to create more than one, as in the previous section, they're seriously inadequate. We have to write out the same code for every object we create, and if we want to change some properties of the object - like adding a `height` property - then we have to remember to update every object.

We would like a way to define the "shape" of an object — the set of methods and the properties it can have — and then create as many objects as we like, just updating the values for the properties that are different.

One way to do this is to use a **constructor**. A constructor is just a function called using the [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) keyword. When you call a constructor, it will:

* create a new object
* bind `this` to the new object, so you can refer to `this` in your constructor code
* run the code in the constructor
* return the new object.

Constructors, by convention, **start with a capital letter** and are named for the type of object they create. For example:

{% code lineNumbers="true" %}
```javascript
function Person(name) {
  this.name = name;
  this.introduceSelf = function () {
    console.log(`Hi! I'm ${this.name}.`);
  };
}
```
{% endcode %}

To call `Person()` as a constructor, we use `new`:

{% code lineNumbers="true" %}
```javascript
const salva = new Person("Salva");
salva.introduceSelf();
// "Hi! I'm Salva."

const frankie = new Person("Frankie");
frankie.introduceSelf();
// "Hi! I'm Frankie."
```
{% endcode %}

## You've been using objects all along <a href="#youve_been_using_objects_all_along" id="youve_been_using_objects_all_along"></a>

Every time we've been working through an example that uses a built-in browser API or JavaScript object, we've been using objects. And **most of the time, these built in objects and APIs create object instances automatically.** For example,

{% code lineNumbers="true" %}
```javascript
myString.split(",");
```
{% endcode %}

You were using a method available on a [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) object. Every time you create a string in your code, that string is **automatically created as an instance of `String`**, and therefore has several common methods and properties available on it.

Note that built in objects and APIs don't always create object instances automatically. As an example, the [Notifications API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API) — which allows modern browsers to fire system notifications — requires you to instantiate a new object instance using the constructor for each notification you want to fire. Try entering the following into your JavaScript console:

{% code lineNumbers="true" %}
```javascript
const myNotification = new Notification("Hello!");
```
{% endcode %}
