# Lab 08 - C Preprocessor

Slides:

{% file src="../../.gitbook/assets/Lab8.pdf" %}
Lab 8 Slides
{% endfile %}

## Exercise 5

1. [#char-cs1010\_read\_line\_arraysize\_t-k](../../past-year-exam/pe1-review/#char-cs1010_read_line_arraysize_t-k "mention") will include the `\n` character.

## C Preprocessing

1. Use `#define` to define a constant to make your program more readable.

### Macro

A _macro_ is a code snippet that is substituted into the program and expanded during pre-processing.

Example:

{% code lineNumbers="true" %}
```c
#include "cs1010.h"
#define SQUARE(x) x * x
int main() {
    // Will output 25
    cs1010_println_long(SQUARE(5));
    // Will output 16.0000
    cs1010_println_double(SQUARE(4.0));
    return 0;
}
```
{% endcode %}

#### Generic Types

We can use a _generic type_ (or _type parameter_) to restrict the type of the arguments used in a macro.

Example:

{% code lineNumbers="true" %}
```c
#include "cs1010.h"
#define SWAP(T, x, y) {T t; t = x; x = y; y = t}
int main() {
    long a = 1;
    long b = 2;
    SWAP(long, a, b); // Now a == 2 && b == 1
    char m[4] = "abc";
    char n[4] = "123";
    SWAP(char *, m, n);
    // Now m is "123" and n is "abc"
    return 0;
}
```
{% endcode %}

#### Pitfall

Be careful with situations like this:

{% code lineNumbers="true" %}
```c
#include "cs1010.h"
#define SQUARE(x) x * x
int main() {
    cs1010_println_long(SQUARE(5 + 1));
    // The above gets expanded to 5 + 1 * 5 + 1
    return 0;
}
```
{% endcode %}

Therefore, we should always use brackets around the arguments of a macro, i.e., `SQUARE(x) (x) * (x)` is safe.

## Bonus Info

1. There are five major types of operations which are core to algorithm optimization: **insertion, removal, retrieval, searching** and **sorting**.

## Searching and Sorting

### Binary Search

1. Probably the **most powerful** search algorithm for simple arrays.
2. The idea of **search space.**
3. **Sorted** means non-descending in CS.

### Comparison-Based Sort

The idea is the pair-wised comparison is important.

#### Bubble Sort

1. There are some better cases when the time complexity is $$O(N)$$.

#### Insertion Sort

1. When the array is sorted, the time complexity is $$O(N)$$

#### Selection Sort

1. The time complexity is always $$O(N)$$

### Counting Sort

1. To use it on negative indices, use the idea of mapping. For example, -9 to 0.

## Exercise 6

1. Start from the minimum point, have two directions.
2. Every time see $$log(n)$$, try thinking about binary search.
3. Every time see $$O(N)$$, which means can done in one iteration.
