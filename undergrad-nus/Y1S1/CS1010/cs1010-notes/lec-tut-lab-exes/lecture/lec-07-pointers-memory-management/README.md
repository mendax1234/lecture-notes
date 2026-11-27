# Lec 07 - Pointers, Memory management

Slides:

{% embed url="https://nus-cs1010.github.io/2425-s1/slides/cs1010-lec07.html#1" %}
Lecture Slides
{% endembed %}

1. Use the "black box" idea to re-think about pointers.
2. Two golden rules
3. `malloc` and `calloc` the idea of "borrow" and heap, refer back to Eldon's explaining with `size_t`.
4. The use of `size_t`, refer back to eldon's explanation
5. String Literal is stored in another section of memory which you won't have access.

## Pointers

### Type Match

When we store a memory address to a pointer, the types must match. Let's consider:

{% code lineNumbers="true" %}
```c
long radius = 5;
double *addr;
addr = &radius; // not ok
```
{% endcode %}

Line 3 above would lead to a **compilation error** since we try to point a `double` pointer to a `long`.

One exception to this rule is the type `void *`. A pointer to `void` has a special place in C, and it can be used to point to any other type without type errors.&#x20;

The `&` operator cannot be used on the left-hand side of the assignment operation. For instance

{% code lineNumbers="true" %}
```c
long x = 1;
long y = 2;
&x = &y; // error
```
{% endcode %}

### Pointer Arithmetic

We can perform arithmetic operations on pointers, but not in the way you expect. Suppose we have a pointer:

{% code lineNumbers="true" %}
```c
long x;
long *ptr;
x = 1;
ptr = &x;
ptr += 1;
```
{% endcode %}

Suppose that `x` is stored in memory address 1000, after Line 4, `ptr` would have the value of 1000. After the line `ptr += 1`, using normal arithmetic operation, we would think that `ptr` will have a value of 1001. However, the semantics for arithmetic operations differ for pointers. The `+` operation for `ptr` causes the `ptr` variable to **move forward by the size of the variable pointed to by the pointer.** In this example, `ptr` points to `long`, assuming that `long` is 8 bytes, after `ptr += 1`, `ptr` will have the value of 1008.

{% hint style="info" %}
We can only do **addition** and **subtraction** for pointers.
{% endhint %}

## Heap

### size\_t

> `size_t` is just a fancy way of saying **non-negative integer.**

While both `size_t` and `long` are integer types, they are not compatible with each other. **Explicit casting** is needed to assign the value of one type to the other.

It is also a common bug to write code like this:

{% code lineNumbers="true" %}
```c
size_t i = 100;
while (i >= 0) {
    // do something to a[i]
    i -= 1;
}
```
{% endcode %}

which loops forever, since `i >= 0` is always true.

## String

### String Literals

A _string literal_ refers to a string written between two `"` characters, such as `"Hello world!"`. And it is stored in a read-only memory region (not the stack).

{% code lineNumbers="true" %}
```c
// Illegal
char *str1 = "Hello!";
str1[5] = '.';

// Legal
char str2[7] = "Hello!";
// or char str2[] = "Hello!"
str2[5] = '.';
```
{% endcode %}

The common between `str1` and `str2` is that both of themselves are **on the stack.** The difference between the two is that `str1` points to a read-only region in the memory (but `str1` itself is a pointer on the stack), while `str2` contains a **copy** of the string on the stack.

{% hint style="info" %}
To create a copy of the string literal on the stack using arrays, we have two methods:

1. `char str[]` or
2. `char str[num]`, where `num` is an integer number

And it is only when we define a pointer that points to the read-only memory region can't we modify its content.
{% endhint %}
