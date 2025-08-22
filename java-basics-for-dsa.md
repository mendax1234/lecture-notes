# Java Basics for DSA

## Data Type

### Numeric Type Conversion

#### Convert between numeric types

As we have seen in CS2030S, Java is a strongly typed language, thus it will do _widening type conversion_  automatically, but _narrowing type conversion_ must be done explicitly.

The syntax for casting a type is to specify the target type in parentheses, followed by the **variable's name** or the **value** to be cast. For example,

```java
System.out.println((int) 1.7);
```

#### String to Numeric Types

From String to `int`, we can use the `parseInt()` provided in the `Integer` class. For example,

{% code lineNumbers="true" %}
```java
String s = "100";
int index = Integer.parseInt(s);
```
{% endcode %}

## Single Dimensional Array

In Java, we create a single dimensional array using the following convention

```
elementType[] arrayRefVar = new elementType[arraySize];
```

For example,

```java
double[] myList = new double[10];
```

### Array size and Default values

The size of array can be accessed by calling the `.length` **field** of an array object.

{% hint style="warning" %}
The size of an array cannot be changed after creation.
{% endhint %}

When an array is created, its elements are assigned the default value of `0` for the numeric primitive data types, `\u0000` for `char` types, and `false` for `boolean` types.

## Multidimensional Array

In Java, we create a multidimensional (two dimensions here) array using the following convention

```
elementType[][] arrayRefVar = new elementType[rowSize][columnSize];
```

For example,

```java
int[][] matrix = new int[10][10];
```

Sometimes, we may want to create a multidimensional array with different column size between each row. This kind of arrays is called **Ragged Array** and its creation is as follows,

{% code lineNumbers="true" %}
```java
int[][] raggedArray = new int[5][];
raggedArray[0] = new int[4];
// Similar for the rest rows
```
{% endcode %}

{% hint style="warning" %}
In Line 1 of the code above, we must specify the first element, which is the size of the row!
{% endhint %}

### Getting Length and Width of a Rectangular Array

This can be done by calling the `.length` property twice

{% code lineNumbers="true" %}
```java
int[][] twodarray = new int[3][4];
int rows = twodarray.length;
int columns = twodarray[0].length;
```
{% endcode %}

### Reading Multi-dimensional Array from the Console

{% code lineNumbers="true" %}
```java
Scanner sc = new Scanner(System.in);
int n = sc.nextInt(); // rows
int m = sc.nextInt(); // columns
sc.nextLine(); // consume the next line after the numbers

char[][] mines = new char[n][m];

for (int i = 0; i < n; i += 1) {
  String line = sc.nextLine();
  for (int j = 0; j < m; j += 1) {
    mines[i][j] = line.charAt(j);
  }
}
sc.close()
```
{% endcode %}

<details>

<summary>Variable-Length Array</summary>

> Don't know whether it's a good practice in Java anot. But at least it's a very bad practice in C 😂

In CS2040S, we can just create the variable length array as we want. For example, if we want to create an array whose size depends on the input, we can do

{% code lineNumbers="true" %}
```java
Scanner sc = new Scanner(System.in);
int n = sc.nextInt();
int[] a = new int[n];
```
{% endcode %}

Similarly, this works for a two-dimensional and multi-dimensional array. Just to remember when you **create** a multi-dimensional array in Java with `new`, you must specify the sizes for **all dimensions except the last one**

</details>

## Java ArrayList

This is a powerful implementation of array in Java!&#x20;

### Create an ArrayList

For example, to create an Integer ArrayList, we can use the following

```java
ArrayList<Integer> Teams = new ArrayList<Integer>();
```

After creating an `ArrayList`, we can treat it as an array! And `ArrayList` has provided many many powerful and useful operations than the normal Java Array.

### Basic Manipulation

{% stepper %}
{% step %}
**Add element**

{% code lineNumbers="true" %}
```java
Teams.add(value);
Teams.add(index, value);
```
{% endcode %}

* The first line appends the specified element (`value`) to the end of the list.
* The second line inserts the specified element (`value`) at the specified position (`index`). And the movement of other elements is all down internally!
{% endstep %}

{% step %}
**Search for element**

```java
Teams.indexOf(value)
```

This line will return the index of the specified element (`value`). This is equal to searching an element in the array!
{% endstep %}

{% step %}
**Remove element**

```java
Teams.remove(index)
```

This line will remove the element at the specified position (`index`). And again, the shifting of other elements are all done internally!
{% endstep %}
{% endstepper %}

## String

`char` in Java is very similar to `char` in C. So, I will just skip it and talk about String only.

String can be declard as follows,

```java
String message = "Welcome to Java";
```

{% hint style="danger" %}
In Java, `String` **cannot** be regarded as `char[]`. Thus using `string[]` is **not allowed**! This is different from[ String in C in CS1010](https://wenbo-notes.gitbook.io/cs1010-notes/lec-tut-lab-exes/lecture/lec-07-pointers-memory-management#string).
{% endhint %}

### Getting String length

This can be done by calling the `.length()` method of a `String` object. For example,

{% include ".gitbook/includes/getting-string-length.md" %}

It will display

```
The length of Welcome to Java is 15
```

### Getting Characters from a String

The `s.charAt(index)` method can be used to retrieve a specific character in a string `s`.

{% hint style="warning" %}
If your `index` is out of the bound, it will cause a `StringIndexOutOfBoundsException`.
{% endhint %}

### Concatenating Strings

Just to save time, we can use the `+` operator. As you have seen below

{% include ".gitbook/includes/getting-string-length.md" %}

To quickly display a numeric value / variable in the `println` statement, we can just put at least one string in the statement and put the numeric value / variable. For example,

{% code lineNumbers="true" %}
```java
String s = "Chapter " + 2;       // s becomes Chapter 2
String y = "Chapter " + 1 + 2;   // y becomes Chapter 12
String z = "Chapter " + (1 + 2); // z becomes Chapter 3
```
{% endcode %}

### Reading a String from the Console

There are two methods to do this,

1. `.next()`: read a string that ends with a whitespace
2. `.nextLine()`: read an entire line of text, as well as the `\n` character.

<details>

<summary>Token-based input and Line-based input</summary>

**Token-based input**: `.next()`, `.nextByte()`, `.nextShort()`, `nextInt()`, `nextLong()`, `nextFloat()`, and `.nextDouble()`. These methods read individual elements separated by **whitespace** characters rather an entire line.

**Line-based input**: `.nextLine()`.

{% hint style="danger" %}
To avoid input errors (usually implicit), **do not** use a line-based input after a token-based input in the program. If you persist on doing so, add one more `.nextLine()` to consume the `\n` character first! Or you can use the following if your line only contains one `int` or other variables of primitive type.

```java
int n = Integer.parseInt(sc.nextLine());
```

This style will help you deal with the `\n` character, so you don't need to use `.nextLine()` again to consume the `\n` character.
{% endhint %}

</details>

### Reading a character from the Console

This is done by read the whole line and retrieve the char at 0 index. For example,

{% code lineNumbers="true" %}
```java
Scanner sc = new Scanner(System.in);
System.out.println("Enter a character: ");
String s = sc.nextLine();
char ch = s.charAt(0);
System.out.println("The character entered is " + ch);
```
{% endcode %}

### String Manipulation with Loops

The normal `String` in Java is **immutable**. So, using conventional method like loops to manipulate on `String` is impossible. Luckily, Java has provided a special class called `StringBuilder`, which makes "String" **mutable**.

For example, if we want to manipulate with each character in a string, which is to change them into lower case.

{% code lineNumbers="true" %}
```java
public String toLowerCase(String s) {
    StringBuilder newS = new StringBuilder(s);
    for (int i = 0; i < s.length(); i += 1) {
        newS.setCharAt(i, Character.toLowerCase(newS.charAt(i)));
    }
    return newS.toString();
}
```
{% endcode %}

{% hint style="danger" %}
`StringBuilder` and `String` are two different types, thus in the `return` statement above, we need to convert `StringBuilder` to `String`.
{% endhint %}

### String Iteration

Given a String `s` / Or your input in Java, the way that Prof Halim does to iterate through it is as follows,

{% code lineNumbers="true" %}
```java
for (char Si : sc.nextLine().toCharArray()) {
    // manipulate with Si
}
```
{% endcode %}

### Getting a substring

Sometimes, we may find that the value we want is the substring of the string we read. For example, the string we get from input is `T100`, but we only want the `100` because this number will be the index. To get the `100`, we can use substring manipulation.

{% code lineNumbers="true" %}
```java
String s = "T100";
String usefulS = s.subString(1);
int index = Integer.parseInt(usefulS);
```
{% endcode %}

In Line 2, `1` is the `beginIndex`, and this will return a String that starts from index 1 and ends at the original end of the original String.

After getting the "useful" substring, you may use the `parseInt()` to convert the string to `int`.

### The great use of `.next()`

As we have seen before, `.next()` is a token-based input. It stops reading when it hits a whitespace. So, if we have a line of strings and we want to read each string, we can use `.next()`. For example,

{% code lineNumbers="true" %}
```java
Scanner sc = new Scanner(System.in);
String si = sc.next();
String sj = sc.next();
sc.nextLine();         // Consume the ending not read \n char
```
{% endcode %}

And if our input is

```
T1 T2\n
```

We will store `T1` in `si` and `T2` in `sj`.

### Replace certain chars in String

To do so, we can use the `.replace()` provided by the Java `String`.

```java
String s = sc.nextLine();
String newS = s.replace("D", ""); // first is target, second is replacement
```
