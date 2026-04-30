# Final (AY20/21)

## Problems

Question paper without answers:

{% file src="../../.gitbook/assets/final-2021-s1.pdf" %}
Question without Answers
{% endfile %}

Question paper with answers

{% file src="../../.gitbook/assets/final-2021-s1-with-comments.pdf" %}
Question with Answers
{% endfile %}

### 3. String Literal

String Literal is stored in a **read-only memory location, which is not stack!!!**

### 9. Illegal Memory Access

In this problem, we can summarise the following four cases when illegal memory access happens:

1. When you return an address on the stack that has been "cleared"

{% code lineNumbers="true" %}
```c
long *bar()
{
    long x = 10;
    return &x;
}
```
{% endcode %}

2. When you try to write to (access) the address stored in an **unintialized pointer**.

{% code lineNumbers="true" %}
```c
long *bar()
{
    long *px;
    *px = 10;
    return px;
}
```
{% endcode %}

3. When you did not allocate enough memory **on the heap** for the variable that a pointer points to. And you try to "access" the pointer by trying to write value into it.

{% code lineNumbers="true" %}
```c
long *bar()
{
    long *px;
    px = (long *)malloc(1);
    *px = 10;
    return px;
}
```
{% endcode %}

4. When you access some address like decimal number 10, etc

{% code lineNumbers="true" %}
```c
long *bar()
{
    return (long *)10;
}
```
{% endcode %}

### 12. String Literal

According to CS1010 Notes,

> A _string literal_ refers to a string written between two `"` characters, such as `"Hello world!"`. And it is stored in a  read-only memory region (not the stack).

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

The difference between the two is that `str1` points to a read-only region in the memory, while `str2` contains a **copy** of the string on the stack.

{% hint style="info" %}
From this question, we can see that to create a copy of the string literal on the stack using arrays, we have two methods:

1. `char str[]` or
2. `char str[num]`, where `num` is an integer number

And it is only when we define a pointer that points to the read-only memory region can't we modify its content.
{% endhint %}

### 13. Time Complexity

The (f) part is similar to AY22/23 Q14. Can compare and study later.

For now, I would say one possible way to reason is to see **how many swap times we need to move the correct number to the correct position**. For example, if our input is 4321, to move 4 to the last, in this algorithm, we need 4 swaps. (Although it is not the case you move 4 all the way to the last like what we have seen in the bubble sort algorithm, the actual case is when you move 4 to a "temp" place, you will move 3 to a "temp" place also, which may be a little bit consuing. But anyway, the total swap times we need is 4 for the number 4). Similarly, for 3, we need 3 moves. So, for the input `n`, we just need the sum from 1 to n, which is $$O(N^2)$$.
