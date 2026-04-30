# Lab 07 - Compiling with Clang

Slides:

{% file src="../../.gitbook/assets/Lab7.pdf" %}
Lab 7 Slides
{% endfile %}

## Exercise 4 Review

### Dynamic Array

1. `malloc()` - memory allocation
2. `calloc(n, m)` - calculate the allocation. And it means allocate a memory space for n values, each of size m.

<details>

<summary>When do should we use <code>malloc()</code>/ dynamic array?</summary>

When we don't know the size of the array at the time we run the program. If we know the size of array, use a static one like `long a[10]` is enough.

</details>

{% hint style="info" %}
Note that `calloc()` will "clear the memory" with 0 before it returns the pointer back. So that is why we are **recommended to use `calloc()`!**
{% endhint %}

### Null Pointer Check

> If you don't do Null Pointer Check, you will be penalised in PE

### 7. Kendall

Use math to simplify the algorithm

```
For each i in permutation:
    for each j before i:
        if j > i:
            count += 1
```

### 9. Subtract

An idea to ignore the leading zero is to use start print from the first nonzero element if the result is not 0.

## Compiler

1. `clang -I`, `-I` stands for "include"

## Multidimensional Arrays

1. Remember the way to allocate a 2-D array (with null pointer check)
2. If the contiguous memory method, the code below

```c
for (long i = 1; i < m; i += 1)
{
    matrix[i] = matrix[i - 1] + n;
}
```

This will define the starting point for each row.

3. Due to array decay, the address of an array is the same as the address of its first element.

## Big O

1. $$f(x) = O(g(x))$$ means f(x) is upper bounded by g(x). or $$f \in O(g)$$, which means set of all functions which do not grow more slowly than f.
2. Include the trivial results about Big O calculation in the cheatsheet.
3. If you have two inputs `m` and `n`, you should include both variables because they are independent.
4. The real way to compare growth rate is to take the limit.&#x20;

## Selected Problems from Exercise 5

