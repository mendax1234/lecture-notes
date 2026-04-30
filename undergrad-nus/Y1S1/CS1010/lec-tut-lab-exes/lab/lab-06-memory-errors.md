# Lab 06 - Memory Errors

Slides:

{% file src="../../.gitbook/assets/Lab6.pdf" %}
Lab 6 Slides
{% endfile %}

## Exercise 3 Review

### Max

The optimization using half lies in the fact that it will reduce the size of the stack you use. It will use less memory.

The runtime is the same because at last you still need to visit each element once. (**Prove it mathematically later)**

### **Padovan**

#### An elegant approach - Circular Array

Store the $$i$$-th Padovan Number (0-based index) into $$list [i~\%~100]$$. Print out the result is interesting.

Access arrays will consume more time because doing pointer arithmetic takes time.

## Memory Management

1. `heap-buffer-overflow` is similar to `index` out of bound for fixed-length array.
2. Cannot free the same memory two times

## Strings

1. String is marked using **double quotation**.
2. The difference between `char *s` and `char s[n]`. The first one is stored in the text region and its element is **read-only**. The second one is a copy of the content stored in the text region that is on the stack.
3. `size_t` is just a fancy way of say a **non-negative** integer.

## Selected Problems from Exercise 4

### Method to iterate through a string

```c
char *s = "hello world";
while (s != '\0')
{
    // do something
    s += 1;
}
```

