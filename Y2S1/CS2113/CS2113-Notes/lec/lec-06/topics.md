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
The algorithms in [`Collections`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Collections.html) are **class methods** (like `Collections.sort`, `Collections.binarySearch`, `Collections.reverse`, etc.), not in the collection interfaces.
{% endhint %}

Below is a table summarizing the interfaces used in Java Collections.

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
   2. `Collections` is a **helper class** with only class/static methods (cannot be instantiated). It provides algorithms mentioned above.
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

### Java File Access

In this part, we will learn how to read from and write to files in Java.

{% stepper %}
{% step %}
#### Represent a File in Java

You can use the [`java.io.File`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/File.html) class to represent a file object. It can be used to access properties of the file object.

{% code lineNumbers="true" %}
```java
import java.io.File;

public class FileClassDemo {

    public static void main(String[] args) {
        File f = new File("data/fruits.txt");
        System.out.println("full path: " + f.getAbsolutePath());
        System.out.println("file exists?: " + f.exists());
        System.out.println("is Directory?: " + f.isDirectory());
    }

}

// This will print out the following
// full path: C:\sample-code\data\fruits.txt
// file exists?: true
// is Directory?: false
```
{% endcode %}

{% hint style="success" %}
#### Code Explanation

1. This code creates a `File` object to represent a file `fruits.txt` that exists in the `data` directory relative to the current working directory and uses that object to print some properties of the file.
2. On Windows, you can just write your path using forward slash "`/`".
{% endhint %}
{% endstep %}

{% step %}
#### Read File as a source of data in `Scanner`

You can read from a file using a `Scanner` object that uses a `File` object as the _source_ of data. For example,

{% code overflow="wrap" lineNumbers="true" %}
```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class FileReadingDemo {

    private static void printFileContents(String filePath) throws FileNotFoundException {
        File f = new File(filePath); // create a File for the given file path
        Scanner s = new Scanner(f); // create a Scanner using the File as the source
        while (s.hasNext()) {
            System.out.println(s.nextLine());
        }
    }

    public static void main(String[] args) {
        try {
            printFileContents("data/fruits.txt");
        } catch (FileNotFoundException e) {
            System.out.println("File not found");
        }
    }

}

// i.e., contents of the data/fruits.txt
// 5 Apples
// 3 Bananas
// 6 Cherries
```
{% endcode %}
{% endstep %}

{% step %}
#### Write to a File

You can use a [`java.io.FileWriter`](https://docs.oracle.com/javase/9/docs/api/java/io/FileWriter.html) object to write to a file. For example,

{% code overflow="wrap" lineNumbers="true" %}
```java
import java.io.FileWriter;
import java.io.IOException;

public class FileWritingDemo {

    private static void writeToFile(String filePath, String textToAdd) throws IOException {
        FileWriter fw = new FileWriter(filePath);
        fw.write(textToAdd);
        fw.close();
    }

    public static void main(String[] args) {
        String file2 = "temp/lines.txt";
        try {
            writeToFile(file2, "first line" + System.lineSeparator() + "second line");
        } catch (IOException e) {
            System.out.println("Something went wrong: " + e.getMessage());
        }
    }

}

// Contents of the temp/lines.txt:
// first line
// second line
```
{% endcode %}

{% hint style="danger" %}
Note that you need to call the `close()` method of the `FileWriter` object for the writing operation to be completed.
{% endhint %}
{% endstep %}

{% step %}
#### Append to a File

You can create a `FileWriter` object that appends to the file (instead of overwriting the current content) by specifying an additional boolean parameter to the constructor. For example,

{% code overflow="wrap" lineNumbers="true" %}
```java
private static void appendToFile(String filePath, String textToAppend) throws IOException {
    FileWriter fw = new FileWriter(filePath, true); // create a FileWriter in append mode
    fw.write(textToAppend);
    fw.close();
}
```
{% endcode %}
{% endstep %}
{% endstepper %}

### Java JAR Files

**Java applications are typically delivered as JAR (short for&#x20;**_**Java Archive**_**) files.** A JAR contains Java classes and other resources (icons, media files, etc.).

* An executable JAR file can be launched using the `java -jar` command e.g., `java -jar foo.jar` launches the `foo.jar` file.
* The IDE or build tools such as Gradle can help you to package your application as a JAR file.

{% hint style="info" %}
For this course, you just need to know how to follow [these steps](https://se-education.org/guides/tutorials/jar.html) to create a JAR file for your ip and tp.
{% endhint %}

### SWE Requirements

A _software requirement_ specifies a need to be fulfilled by the software product. And a software project may be,

* **a&#x20;**_**brownfield**_**&#x20;project** e.g., develop a product to replace/update an existing software product
* **a&#x20;**_**greenfield**_**&#x20;project** e.g., develop a totally new system from scratch

In either case, requirements need to be gathered, analyzed, specified, and managed. And requirements come from _stakeholders_.

> **Stakeholder** is an individual or an organization that is involved or potentially affected by the software project. e.g., users, sponsors, developers, interest groups, government agencies, etc.

Requirements can be divided into two in the following way:

1. _**Functional requirements**_ specify what the system should do.
2. [_**Non-functional requirements**_](topics.md#non-functional-requirement) _**(NFR)**_ specify the constraints under which the system is developed and operated.

#### **Non-functional Requirement**

Below are some examples of non-functional requirement categories:

* Data requirements e.g. size, volatility, persistency etc.,
* Environment requirements e.g. technical environment in which the system would operate in or needs to be compatible with.
* Accessibility, Capacity, Compliance with regulations, Documentation, Disaster recovery, Efficiency, Extensibility, Fault tolerance, Interoperability, Maintainability, Privacy, Portability, Quality, Reliability, Response time, Robustness, Scalability, Security, Stability, Testability, and more ...

{% hint style="warning" %}
[User story](https://wenbo-notes.gitbook.io/cs2113swe-notes/lec/lec-05/topics#swe-user-story) is **function requirement**, not NFR!&#x20;
{% endhint %}

#### Functional Requirement

As the name suggests and the definition above, these are just the technical stuff, like [user story](https://wenbo-notes.gitbook.io/cs2113swe-notes/lec/lec-05/topics#swe-user-story).

### SWE Use Case

> Don't confuse use case with [user story](https://wenbo-notes.gitbook.io/cs2113swe-notes/lec/lec-05/topics#swe-user-story).

**Use case** is a description of a set of sequences of actions, including variants, that a system performs to yield an observable result of value to an actor. Thus,

* A use case describes an _interaction between the user and the system_ for a _specific functionality_ of the system, and
* it captures the _functional requirements_ of a system.

### [RCS Merge Conflicts](https://wenbo-notes.gitbook.io/cs2113swe-notes/lec/lec-05/topics#resolving-merge-conflicts)

This part is totally the same as last week's content, maybe should remind the course team to update :joy:

### SWE Code Quality: Unsafe Practices

**It is safer to use language constructs in the way they are meant to be used**, even if the language allows shortcuts. Such coding practices are common sources of bugs. Know them and avoid them. These practice includes

{% stepper %}
{% step %}
#### `case` statement

1. Always include a default branch in `case` statements.
2. Furthermore, use the `default` branch for the intended default action and not just to execute the last option.
{% endstep %}

{% step %}
#### Don't recycle variables or parameters

* Use one variable for one purpose.
* Do not _reuse_ formal parameters as local variables inside the method.

For example,

{% code lineNumbers="true" %}
```java
// Bad practice
double computeRectangleArea(double length, double width) {
    length = length * width;  // parameter reused as a variable
    return length;
}

// Good practice
double computeRectangleArea(double length, double width) {
    double area;
    area = length * width;
    return area;
}
```
{% endcode %}
{% endstep %}
{% endstepper %}

## Classic Questions

{% stepper %}
{% step %}
#### Non-functional Requirements

Given below are some requirements of TEAMMATES (an online peer evaluation system for education). Which one of these are non-functional requirements?

* [x] &#x20;a. The response to any use action should become visible within 5 seconds.
* [ ] &#x20;b. The application admin should be able to view a log of user activities.
* [x] &#x20;c. The source code should be open source.
* [x] &#x20;d. A course should be able to have up to 2000 students.
* [ ] &#x20;e. As a student user, I can view details of my team members so that I can know who they are.
* [x] &#x20;f. The user interface should be intuitive enough for users who are not IT-savvy.
* [x] &#x20;g. The product is offered as a free online service.

***

**Explanation**: (b) are (e) are functions available for a specific user types. Therefore, they are functional requirements. (a), (c), (d), (f) and (g) are either constraints on functionality or constraints on how the project is done, both of which are considered non-functional requirements.
{% endstep %}
{% endstepper %}
