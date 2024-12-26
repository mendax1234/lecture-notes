# The web standards model

## Brief history of the web

We'll keep this very brief, as there are many (more) detailed accounts of the web's history out there, which we'll link to later on (also try searching for "history of the web" in your favorite search engine and see what you get, if you are interested in more detail.)

In the late 1960s, the US military developed a communication network called [ARPANET](https://developer.mozilla.org/en-US/docs/Glossary/Arpanet). This can be considered a forerunner of the Web, as it worked on [packet switching](https://en.wikipedia.org/wiki/Packet_switching), and featured the first implementation of the [TCP/IP](https://en.wikipedia.org/wiki/Internet_protocol_suite) protocol suite. These two technologies form the basis of the infrastructure that the internet is built on.

In 1980, Tim Berners-Lee (often referred to as TimBL) wrote a notebook program called ENQUIRE, which featured the concept of links between different nodes. Sound familiar?

Fast forward to 1989, and TimBL wrote [Information Management: A Proposal](https://www.w3.org/History/1989/proposal.html) and HyperText at CERN; these two publications together provided the background for how the web would work. They received a fair amount of interest, enough to convince TimBL's bosses to allow him to go ahead and create a global hypertext system.

By late 1990, TimBL had created all the things needed to run the first version of the web — [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP), [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML), the first web browser, which was called [WorldWideWeb](https://en.wikipedia.org/wiki/WorldWideWeb), an HTTP server, and some web pages to look at.

In the next few years that followed, the web exploded, with multiple browsers being released, thousands of web servers being set up, and millions of web pages being created. OK, that's a very simple summary of what happened, but we did promise you a brief summary.

One last significant data point to share is that in 1994, TimBL founded the [World Wide Web Consortium](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium) (W3C), an organization that brings together representatives from many different technology companies to work together on the creation of web technology specifications. After that other technologies followed such as [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) and [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript), and the web started to look more like the web we know today.

## Web standards

**Web standards** are the technologies we use to build websites. These standards exist as long technical documents called specifications, which detail exactly how the technology should work. These documents are not very useful for learning how to use the technologies they describe (this is why we have sites like MDN Web Docs), but instead are intended to be used by software engineers to implement these technologies (usually in web browsers).

For example, the [HTML Living Standard](https://html.spec.whatwg.org/multipage/) describes exactly how HTML (all the HTML elements, and their associated APIs, and other surrounding technologies) should be implemented.

Web standards are created by standards bodies — institutions that invite groups of people from different technology companies to come together and agree on how the technologies should work in the best way to fulfill all of their use cases. The W3C is the best known web standards body, but there are others such as the [WHATWG](https://whatwg.org/) (who maintain the living standards for the HTML language), [ECMA](https://ecma-international.org/) (who publish the standard for ECMAScript, which JavaScript is based on), [Khronos](https://www.khronos.org/) (who publish technologies for 3D graphics, such as WebGL), and others.

### Web standard key principles

The basic principles of the web:

* Open to contribute and use.
* Not patent-encumbered or controlled by a single private entity.
* Accessible and interoperable.
* They don't break the web.

This basis means that the web is a unique and exciting industry to get involved in.

### "Open" standards

One of the key aspects of web standards, which TimBL and the W3C agreed on from the start, is that the web (and web technologies) should be free to both contribute and use, and not encumbered by patents/licensing. Therefore anyone can write the code to build a website for free, and anyone can contribute to the standards creation process, where the specs are written.

Because web technologies are created openly, in collaboration between many different companies, it means that no one company gets to control them, which is a really good thing. You wouldn't want a single company suddenly deciding to put the entire web behind a paywall, or releasing a new version of HTML that everyone has to buy to continue making websites, or worse still, just deciding they aren't interested any more and just turning it off.

This allows the web to remain a freely-available public resource.

### Don't break the web

Another phrase you'll hear around open web standards is "don't break the web" — the idea is that any new web technology that is introduced should be **backwards compatible** with what went before it (i.e. old websites will still continue to work), and **forwards compatible** (future technologies in turn will be compatible with what we currently have). As you go through the learning material presented here, you'll start to learn how this is made possible with some very clever design and implementation work.

## Overview of modern web technologies

There are a number of technologies to learn if you want to be a front-end web developer. In this section we will describe them briefly.

### HTML, CSS, and JavaScript

[HTML](../frontend/html/), [CSS,](../frontend/css.md) and [JavaScript](../frontend/javascript/) are the main three technologies you'll use to build a website.

* HTML is for structure and semantics (meaning).
* CSS is for styling and layout.
* JavaScript is for controlling dynamic behavior.

#### HTML

If we adopted a house-building analogy, HTML would be like the foundations and walls of the house, which give it structure and hold it together.

#### CSS

In the house analogy, CSS is like the paint, wallpaper, carpets and paintings you'd use to make the house look nice.

#### JavaScript

In the house analogy, JavaScript is like the cooker, TV, Microwave, or hairdryer — the things that give your house useful functionality.

### Server-side languages and frameworks

HTML, CSS, and JavaScript are front-end (or client-side) languages, which means they are run by the browser to produce a website front-end that your users can use.

There are another class of languages called back-end (or server-side) languages, meaning that they are run on the server before the result is then sent to the browser to be displayed. A typical use for a server-side language is to get some data out of a database and generate some HTML to contain the data, before then sending the HTML over to the browser to display it to the user.

Example server-side frameworks include ASP.NET (in C#), Django (in Python), Laravel (in PHP), and Next.js (in JavaScript).

