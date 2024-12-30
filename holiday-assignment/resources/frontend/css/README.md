# CSS

CSS (Cascading Style Sheets) is the code that styles web content. _Styling the content_ walks through what you need to get started. We'll answer questions like: How do I make text red? How do I make content display at a certain location in the (webpage) layout? How do I decorate my webpage with background images and colors?

## CSS Basic

> This part is mainly from MDN [CSS Crash Course](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Your_first_website/Styling_the_content). I found it pretty useful and suitable for beginners.

Like HTML, CSS is not a programming language. It's not a markup language either. **CSS is a style sheet language.** CSS is what you use to **selectively style HTML elements**. For example, this CSS selects paragraph text, setting the color to red:

{% code lineNumbers="true" %}
```css
p {
  color: red;
}
```
{% endcode %}

### CSS Ruleset

Let's dissect the CSS code for red paragraph text to understand how it works:

<figure><img src="../../../../.gitbook/assets/css-ruleset.png" alt=""><figcaption></figcaption></figure>

The whole structure is called a **ruleset**. (The term _ruleset_ is often referred to as just _rule_.) Note the names of the individual parts:

1. **Selector:** This is the HTML element name at the start of the ruleset. It defines the element(s) to be styled (in this example, [`<p>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p) elements). To style a different element, change the selector.
2. **Declaration**: This is a single rule like `color: red;`. It specifies which of the element's **properties** you want to style.
3. **Properties**: These are features of an HTML element that you can change the values of, to make it styled differently. (In this example, `color` is a property of the [`<p>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p) elements.) In CSS, you choose which properties you want to affect in the rule.
4. **Property value**: To the right of the property — after the colon — there is the **property value**. This chooses one out of many possible appearances for a given property. (For example, there are many `color` values in addition to `red`.)

{% hint style="info" %}
Note the other important parts of the syntax:

* Apart from the selector, each ruleset must be wrapped in curly braces. (`{}`)
* Within each declaration, you must use a colon (`:`) to separate the property from its value or values.
* Within each ruleset, you must use a semicolon (`;`) to separate each declaration from the next one.
{% endhint %}

To modify multiple property values in one ruleset, write them separated by semicolons, like this:

{% code lineNumbers="true" %}
```css
p {
  color: red;
  width: 500px;
  border: 1px solid black;
}
```
{% endcode %}

#### Selecting Multiple Elements

You can also select multiple elements and apply a single ruleset to all of them. Separate multiple selectors by commas. For example:

{% code lineNumbers="true" %}
```css
p,
li,
h1 {
  color: red;
}
```
{% endcode %}

#### Different Types of selectors

There are many different types of selectors. The examples above use **element selectors**, which select all elements of a given type. But we can make more specific selections as well. Here are some of the more common types of selectors:

| Selector name                                              | What does it select                                                                                              | Example                                                                                                                    |
| ---------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Element selector (sometimes called a tag or type selector) | All HTML elements of the specified type.                                                                         | <p><code>p</code><br>selects <code>&#x3C;p></code></p>                                                                     |
| ID selector                                                | The element on the page with the specified ID. **On a given HTML page, each id value should be unique.**         | <p><code>#my-id</code><br>selects <code>&#x3C;p id="my-id"></code> or <code>&#x3C;a id="my-id"></code></p>                 |
| Class selector                                             | The element(s) on the page with the specified class. Multiple instances of the same class can appear on a page.  | <p><code>.my-class</code><br>selects <code>&#x3C;p class="my-class"></code> and <code>&#x3C;a class="my-class"></code></p> |
| Attribute selector                                         | The element(s) on the page with the specified attribute.                                                         | <p><code>img[src]</code><br>selects <code>&#x3C;img src="my-image.png"></code> but not <code>&#x3C;img></code></p>         |
| Pseudo-class selector                                      | The specified element(s), but only when in the specified state. (For example, when a cursor hovers over a link.) | <p><code>a:hover</code><br>selects <code>&#x3C;a></code>, but only when the mouse pointer is hovering over the link.</p>   |

### CSS: All about boxes

Something you'll notice about CSS as you use it more: a lot of it is about boxes. This includes setting size, color, and position. Most HTML elements on your page can be thought of as boxes sitting on top of other boxes.

<figure><img src="../../../../.gitbook/assets/css-box-anatomy.png" alt=""><figcaption></figcaption></figure>

CSS layout is mostly based on the _box model._ Each box taking up space on your page has properties like:

* `padding`, the space around the content. In the example below, it is the space around the paragraph text.
* `border`, the solid line that is just outside the padding.
* `margin`, the space around the outside of the border.

<figure><img src="../../../../.gitbook/assets/css-box-model.png" alt=""><figcaption></figcaption></figure>

### How is CSS applied to HTML?

When a browser displays a document, it must combine the document's content with its style information. It processes the document in a number of stages, which we've listed below. Bear in mind that this is a very simplified version of what happens when a browser loads a webpage, and that different browsers will handle the process in different ways.

1. The browser loads the HTML.
2. It converts the HTML into a tree-like structure called a [DOM](https://developer.mozilla.org/en-US/docs/Glossary/DOM) (_Document Object Model_) tree. The DOM represents the document in the computer's memory.
3. The browser then fetches most of the resources that are linked to by the HTML document, such as embedded images, videos, and CSS! JavaScript is handled a bit later on in the process, and we won't talk about it here to keep things simpler.
4. The browser parses the fetched CSS, and sorts the different rules into different "buckets" based on which HTML elements (represented in the DOM as **nodes**) they will be applied to. The browser then attaches styles to different elements as required (this intermediate step is called a **render tree**).
5. The render tree is laid out in the structure it should appear in after the rules have been applied.
6. The visual display of the page is shown on the screen (this stage is called painting).

The following diagram also offers a simple view of the process.

<figure><img src="../../../../.gitbook/assets/css-basic-load-website-diagram.png" alt=""><figcaption></figcaption></figure>

#### More on the DOM

As mentioned above, the DOM has a tree-like structure. Each element, attribute, and piece of text in the markup language becomes a [DOM node](https://developer.mozilla.org/en-US/docs/Glossary/Node/DOM) in the tree structure. The nodes are defined by their relationship to other DOM nodes. Some elements are parents of child nodes, and child nodes have siblings.

Understanding the DOM will help you design, debug, and maintain your CSS because the DOM is where your CSS and the document's content meet up. When you use browser DevTools you navigate the DOM as you select items in order to see which rules apply.

Let's look at an example to see how a real HTML snippet is converted into a DOM. Take the following HTML code:

{% code lineNumbers="true" %}
```html
<p>
  Let's use:
  <span>Cascading</span>
  <span>Style</span>
  <span>Sheets</span>
</p>
```
{% endcode %}

The browser parses the HTML and creates a DOM from it, which looks like this:

{% code lineNumbers="true" %}
```
P
├─ "Let's use:"
├─ SPAN
|  └─ "Cascading"
├─ SPAN
|  └─ "Style"
└─ SPAN
    └─ "Sheets"
```
{% endcode %}

In the DOM, the node corresponding to our `<p>` element is a parent. Its children are a text node and the three nodes corresponding to our `<span>` elements. The `SPAN` nodes are also parents, with text nodes as their children. When the browser renders the above DOM tree, it will look like so:

<figure><img src="../../../../.gitbook/assets/css-basic-more-on-dom.png" alt=""><figcaption></figcaption></figure>

#### Applying CSS to the DOM

Let's say we modify our code to apply the following CSS to our document, to style it:

{% code lineNumbers="true" %}
```css
span {
  border: 1px solid black;
  background-color: lime;
}
```
{% endcode %}

In this case, the browser will first create a DOM tree, as in the previous section. Next, it parses the CSS. Since the only rule available in the CSS has a `span` selector, the browser sorts the CSS very quickly! It applies that rule to each one of the three `<span>`s, then paints the final visual representation to the screen.

The updated output is as follows:

<figure><img src="../../../../.gitbook/assets/css-basic-more-on-dom-css-updated.png" alt=""><figcaption></figcaption></figure>

## CSS Advanced

To quickly master CSS, you still need to know the following:

* **All CSS Properties and their acceptable values**: This is the basic founding stone for CSS and fortunately, MDN has provided a quite detailed [CSS reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference).
* [css-selectors.md](css-selectors.md "mention")
  * [**Basic CSS Selectors**](css-selectors.md#basic-css-selectors)
  * [**Attribute Selectors**](css-selectors.md#attribute-selectors)
  * [**Pseudo-class and Pseudo-element**](css-selectors.md#pseudo-classes-and-pseudo-elements)
* **Combinators**

## Learning Resources

* [CSS Crash Course](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Your_first_website/Styling_the_content) by MDN
