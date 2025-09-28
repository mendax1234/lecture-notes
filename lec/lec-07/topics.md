# Topics

## Important Points

### Java Varargs

> This syntactic sugar has appeared [CS2030S](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-07-immutability-and-nested-classes/diagnostic-quiz#id-05.-...-and-safevarargs)!

Variable Arguments (Varargs) is a _syntactic sugar_ type feature that allows writing a method that can take a variable number of arguments. For example,

{% code lineNumbers="true" %}
```java
public static void search(String ... keywords) {
   // method body
}
```
{% endcode %}

{% hint style="success" %}
#### Code Explanation

The `search` method below can be called as `search()`, `search("book")`, `search("book", "paper")`, etc.
{% endhint %}

### Java Streams

> Again, this is the emphasis of CS2030S, but unfortunately not CS2113. FYI, please go to my [CS2030S notes](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-09-infinitelist-and-stream) to know more about it!

### Java FX

JavaFX is a technology for building Java-based GUIs. Previously it was a part of Java itself, but has become a third-party dependency since then. It is now being maintained by [OpenJDK](https://wiki.openjdk.java.net/display/OpenJFX).

{% hint style="info" %}
As this is not the emphasis in CS2113, please refer to the [_JavaFX tutorial_ @SE-EDU/guides](https://se-education.org/guides/tutorials/javaFx.html) to learn how to get started with JavaFX.
{% endhint %}

### JavaDoc

JavaDoc is a tool for generating API documentation in HTML format from comments in the source code. In CS2113, you only need to follow the following examples to write your JavaDoc

{% tabs %}
{% tab title="For methods" %}
{% code lineNumbers="true" %}
```java
/**
 * Returns lateral location of the specified position.
 * If the position is unset, NaN is returned.
 *
 * @param x X coordinate of position.
 * @param y Y coordinate of position.
 * @param zone Zone of position.
 * @return Lateral location.
 * @throws IllegalArgumentException If zone is <= 0.
 */
public double computeLocation(double x, double y, int zone)
    throws IllegalArgumentException {
    // ...
}
```
{% endcode %}
{% endtab %}

{% tab title="For classes" %}
{% code overflow="wrap" lineNumbers="true" %}
```java
package ...

import ...

/**
 * Represents a location in a 2D space. A <code>Point</code> object corresponds to
 * a coordinate represented by two integers e.g., <code>3,6</code>
 */
public class Point {
    // ...
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Code Quality
