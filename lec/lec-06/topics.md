# Topics

## Important Points

### Java Generics

> I believe CS2030S has a very good explanation about [generics](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-05-generics)! Feel free to go back and review it.

**Generics enable types (classes and interfaces) to be parameters when defining classes, interfaces and methods.** Much like the more familiar _formal parameters_ used in method declarations, _type parameters_ provide a way for you to re-use the same code with different inputs. The difference is that the inputs to formal parameters are values, while the inputs to type parameters are types.

{% hint style="success" %}
The comparison of formal parameters and type parameters here is awesome!
{% endhint %}

### Java Collections

A _collection_ — sometimes called a _container_ — is simply an object that groups multiple elements into a single unit. Collections are used to store, retrieve, manipulate, and communicate aggregate data.

**The collections framework is a unified architecture for representing and manipulating collections.** It contains the following:

* **Interfaces**: These are abstract data types that represent collections. Interfaces allow collections to be manipulated independently of the details of their representation.
  * Example: the `List<E>` interface can be used to manipulate list-like collections which may be implemented in different ways such as `ArrayList<E>` or `LinkedList<E>`.
* **Implementations**: These are the concrete implementations of the collection interfaces. In essence, they are reusable data structures.
  * Example: the `ArrayList<E>` class implements the `List<E>` interface while the `HashMap<K, V>` class implements the `Map<K, V>` interface.
* **Algorithms**: These are the methods that perform useful computations, such as _searching_ and _sorting_, on objects that implement collection interfaces. The algorithms are said to be polymorphic: that is, the same method can be used on many different implementations of the appropriate collection interface.
  * Example: the `sort(List<E>)` method can sort a collection that implements the `List<E>` interface.

{% hint style="warning" %}
The algorithms are in `Collections` **class methods** (like `Collections.sort`, `Collections.binarySearch`, `Collections.reverse`, etc.), not in the collection interfaces.
{% endhint %}

| Interface                                                                                                    | Extends    | Characteristics                                                                                    | Allows Duplicates?                    | Ordered?                                                |
| ------------------------------------------------------------------------------------------------------------ | ---------- | -------------------------------------------------------------------------------------------------- | ------------------------------------- | ------------------------------------------------------- |
| [**Collection\<E>**](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Collection.html) | –          | Root interface for all collections of objects. Defines basic operations (add, remove, size, etc.). | Depends on subtype                    | Depends                                                 |
| [**List\<E>**](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/List.html)             | Collection | Ordered sequence, index-based access, insertion at specific positions.                             | ✅ Yes                                 | ✅ Yes                                                   |
| [**Set\<E>**](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Set.html)               | Collection | Models a mathematical set; no duplicate elements allowed.                                          | ❌ No                                  | ❌ Not necessarily                                       |
| [**SortedSet\<E>**](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/SortedSet.html)   | Set        | Maintains elements in ascending order according to natural order or comparator.                    | ❌ No                                  | ✅ Yes                                                   |
| [**Queue\<E>**](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Queue.html)           | Collection | Holds elements prior to processing (FIFO typically, but can vary).                                 | ✅ Yes (depends on implementation)     | ✅ Often                                                 |
| [**Deque\<E>**](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Deque.html)           | Queue      | Double-ended queue; supports insertion/removal at both ends.                                       | ✅ Yes                                 | ✅ Yes                                                   |
| [**Map\<K,V>**](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Map.html)             | –          | Key-value pairs, unique keys. Each key maps to at most one value.                                  | <p>Keys: ❌ No</p><p>Values: ✅ Yes</p> | ✅ Depends (e.g., `HashMap` unordered, `TreeMap` sorted) |
| [**SortedMap\<K,V>**](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/SortedMap.html) | Map        | Maintains mappings in ascending key order.                                                         | Keys: ❌ No                            | ✅ Yes                                                   |

{% hint style="warning" %}
#### Notes

1. One example of the collections framework mentioned above is C++ STL.
2. `Collection` and `Collections` are different!
   1. `Collection` is an interface
   2. `Collections` is a **helper class** with only static methods (cannot be instantiated). It provides algorithms mentioned above.
{% endhint %}

#### HashMap Example

> This content actually can serve as a good reference for CS2040S!

[`HashMap`](https://wenbo-notes.gitbook.io/cs2040s-notes/lec-lab/lec-06-hash-table) is an implementation of the `Map` interface. It allows you to store a collection of _key-value pairs_.

{% code lineNumbers="true" %}
```java
import java.awt.Point;
import java.util.HashMap;
import java.util.Map;

public class HashMapDemo {
    public static void main(String[] args) {
        HashMap<String, Point> points = new HashMap<>();

        // put the key-value pairs in the HashMap
        points.put("x1", new Point(0, 0));
        points.put("x2", new Point(0, 5));
        points.put("x3", new Point(5, 5));
        points.put("x4", new Point(5, 0));

        // retrieve a value for a key using the get method
        print("Coordinates of x1: " + pointAsString(points.get("x1")));

        // check if a key or a value exists
        print("Key x1 exists? " + points.containsKey("x1"));
        print("Key y1 exists? " + points.containsKey("y1"));
        print("Value (0,0) exists? " + points.containsValue(new Point(0, 0)));
        print("Value (1,2) exists? " + points.containsValue(new Point(1, 2)));

        // update the value of a key to a new value
        points.put("x1", new Point(-1,-1));

        // iterate over the entries
        for (Map.Entry<String, Point> entry : points.entrySet()) {
            print(entry.getKey() + " = " + pointAsString(entry.getValue()));
        }

        print("Number of keys: " + points.size());
        points.clear();
        print("Number of keys after clearing: " + points.size());

    }

    public static String pointAsString(Point p) {
        return "[" + p.x + "," + p.y + "]";
    }

    public static void print(String s) {
        System.out.println(s);
    }
}
```
{% endcode %}

{% hint style="success" %}
#### Code Explanation

The example above illustrates how to use a `HashMap<String, Point>` to maintain a list of coordinates and their identifiers e.g., the identifier `x1` is used to identify the point `0,0` where `x1` is the _key_ and `0,0` is the _value_.
{% endhint %}
