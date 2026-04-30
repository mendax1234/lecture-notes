# Diagnostic Quiz

## Problems

### 1. Tower of Hanoi

> _Disk `i-1` is moved twice the number of times of Disk `i`_

It is **correct.** This can be reasoned using the recurrence relation $$T(n)=2T(n-1)+1$$ for the Tower of Hanoi problem.

### 5. Time Complexity for Nqueens

> _Consider the solution to N-Queens given in class. Suppose the running time of `nqueens` is_ $$T(n)$$_. What is_ $$T(1)$$_?_

This is an awesome question. $$T(1)$$ happens when we reach the **base case**, so here we need to think about what will happen in the **base case**.

The base case is:

{% code lineNumbers="true" %}
```c
if (row == n - 1) {
  if (!threaten_each_other_diagonally(queens, n - 1)) {
    cs1010_println_string(queens);
    return true;
  }
  return false;
}
```
{% endcode %}

It calls `threaten_each_other_diagonally` which runs in $$O(n)$$.

```c
bool threaten_each_other_diagonally(char queens[], size_t last_row) {
  for (size_t curr_row = 0; curr_row <= last_row; curr_row += 1) {
    if (has_a_queen_in_diagonal(queens, curr_row, last_row)) {
      return true;
    }
  }
  return false;
}
```

And print a string with length `n` takes $$O(n)$$. So, the overall running time for $$T(1)$$ is $$O(n^2)$$.
