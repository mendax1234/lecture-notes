# Diagnostic Quiz

## Problems

### 3. Recursive linear seach

The question itself is trivial, it mainly wants to test you whether the two bounds are **inclusive** or not.

For the third option

{% code lineNumbers="true" %}
```c
// search for q in list[i] .. list[j-1]
long search(const long list[], long i, long j, long q) {
  if (i > j) {
    return -1;
  }
  long mid = (i + j)/2;
  if (list[mid] == q) {
    return mid;
  }
  long found = search(list, i, mid, q); // search in list[i]..list[mid-1]
  if (found >= 0) {
    return found;
  }
  return search(list, mid, j, q); // search in list[mid]..list[j-1]
}

// Call with
search(list, 0, n, q);
```
{% endcode %}

Since `n` should be exclusive, so this option is incorrect.

### 13. The best case runtime for insertion sort

Given that a list is sorted in **increasing order** already. The running to for insertion sort will be $$O(n)$$.
