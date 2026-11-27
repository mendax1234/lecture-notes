# Diagnostic Quiz

## Problems

### 8. Compilation Error vs. Runtime Error

> Suppose we have the following array declaration:
>
> ```c
> long per_burger[5] = {3, 2, 1, 15, 5};
> ```
>
> Try out the following code.  Which one would generate a compilation **error**?

See the difference between compilation error and runtime error at [#compilation-error-vs.-run-time-error](../lec-03-basic-c-programming.md#compilation-error-vs.-run-time-error "mention") in Lec 03.

### 11. `const` in C

> Consider the function declaration below:
>
> ```c
> void bar(const long a[]) { }
> ```
>
> Which of the following statement is FALSE?

In C, the `const` keyword applies to the thing **immediately to its left** (or, if there's nothing to the left, it applies to the thing to its right).

So, in this problem, it means we cannot change any element in the array `a`. But since `a` is also a pointer variable (See more explanation [here](https://piazza.com/class/lz3qhq0epwf53k/post/103) about why it is not merely an address due to array decay), we can set it to another array.

`const long* a` means `a` is a pointer to const, and any attempt to write via `a` will error out. e.g. `*a=10`is invalid.

This is different from `long* const a`, which declares a const pointer `a` that **cannot** be reassigned to point to another place. e.g. `a=<pointer>` is invalid.

## Tips

1. A stack frame stores **function parameters** and **local variables.**
2. When you access the "out of bound" index of an array, it won't generate **compilation error**, but it may generate **runtime error** or **undefined behavior**.
