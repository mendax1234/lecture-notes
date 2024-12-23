# HTML Basic

> This part is mainly from MDN [HTML Crash Course](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content). I found it pretty useful and suitable for beginners.

HTML (**H**yper**T**ext **M**arkup **L**anguage) is the code that is used to structure a web page and its content. For example, content could be structured within a set of paragraphs, a list of bulleted points, or using images and data tables.

## What is HTML?

HTML is a _markup language_ that defines the structure of your content. HTML consists of a series of [**elements**](https://developer.mozilla.org/en-US/docs/Glossary/Element), which you use to enclose, or wrap, different parts of the content to make it appear a certain way, or act a certain way. The enclosing [tags](https://developer.mozilla.org/en-US/docs/Glossary/Tag) can make a word or image hyperlink to somewhere else, can italicize words, can make the font bigger or smaller, and so on. For example, take the following line of content:

```
My cat is very grumpy
```

If we wanted the line to stand by itself, we could specify that it is a paragraph by enclosing it in paragraph tags:

```html
<p>My cat is very grumpy</p>
```

## Element <a href="#anatomy_of_an_html_element" id="anatomy_of_an_html_element"></a>

Let's explore this paragraph element a bit further.

<figure><img src="../../../../.gitbook/assets/html-element.png" alt=""><figcaption></figcaption></figure>

The main parts of our element are as follows:

1. **The opening tag:** This consists of the name of the element (in this case, `p`), wrapped in opening and closing **angle brackets**. This states where the element begins or starts to take effect — in this case where the paragraph begins.
2. **The closing tag:** This is the same as the opening tag, except that it includes a _forward slash_ before the element name. This states where the element ends — in this case where the paragraph ends. Failing to add a closing tag is one of the standard beginner errors and can lead to strange results.
3. **The content:** This is the content of the element, which in this case, is just text.
4. **The element:** The opening tag, the closing tag, and the content together comprise the element.

{% hint style="info" %}
Note that here the element's name is `p`, which represents paragraph.&#x20;
{% endhint %}

## Attributes

Elements can also have attributes that look like the following:

<figure><img src="../../../../.gitbook/assets/html-element-with-attribute.png" alt=""><figcaption></figcaption></figure>

Attributes contain extra information about the element that you don't want to appear in the actual content. Here, `class` is the attribute _name_ and `editor-note` is the attribute _value_. The `class` attribute allows you to give the element a non-unique identifier that can be used to target it (and any other elements with the same `class` value) with style information and other things. Some attributes have no value, such as [`required`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/required).

Attributes that set a value always have:

1. A space between it and the element name (or the previous attribute, if the element already has one or more attributes).
2. The attribute name followed by an equal sign.
3. The attribute value wrapped by opening and closing quotation marks.

## Void Elements

Some elements have no content and are called [**void elements**](https://developer.mozilla.org/en-US/docs/Glossary/Void_element). Take the [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) element that we already have in our HTML page:

{% code lineNumbers="true" %}
```javascript
<img src="images/firefox-icon.png" alt="My test image" />
```
{% endcode %}

This contains two attributes, but there is no closing `</img>` tag and no inner content. This is because an image element doesn't wrap content to affect it. Its purpose is to embed an image in the HTML page in the place it appears.

## Learning Resources

* [HTML Crash Course](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content) by MDN
