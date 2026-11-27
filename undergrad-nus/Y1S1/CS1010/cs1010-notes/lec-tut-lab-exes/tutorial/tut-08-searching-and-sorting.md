# Tut 08 - Searching and Sorting

## Problem Set 21

### Problem 21.1

> The Loop version of Binary Search

{% code lineNumbers="true" %}
```c
long search_loop(const long list[], long i, long j, long q) {
    while(i<=j) {
        long mid = (i+j)/2;
        if (list[mid] == q) {
            return mid;
        }
        if (list[mid] > q) {
            j = mid - 1;
            continue;
        }
        i = mid + 1;
    }
    return -1;
}
```
{% endcode %}

### Problem 21.2\*

Question: Instead of returning -1 if the query `q` is not found, modify the binary search algorithm in Problem 21.1 such that it returns either:

* a position `k`, such that `list[k] <= q <= list[k+1]`.
* `-1` if `q < list[0]`
* `n - 1` if `q > list[n-1]`

{% code lineNumbers="true" %}
```c
long search_loop(const long list[], long i, long j, long q) {
    while(i<=j) {
        long mid = (i+j)/2;
        if (list[mid] == q) {
            return mid;
        }
        if (list[mid] > q) {
            j = mid - 1;
            continue;
        }
        i = mid + 1;
    }
    return j;
}
```
{% endcode %}

The reason to `return mid` is easy to understand. But `return j` at last needs some pattern recognition skill. :joy:

## Problem Set 22

### Problem 22.1\*

> Optimize the Bubble Sort.

Basically, when there is not swap, which means our list is sorted already.

{% code lineNumbers="true" %}
```c
void swap(long a[], size_t i, size_t j) {
    long temp = a[i];
    a[i] = a[j];
    a[j] = temp;
}

bool bubble_pass(size_t last, long a[]) {
    bool has_swapped = false;
    for (size_t i = 0; i < last; i += 1) {
        if (a[i] > a[i+1]) {
            swap(a, i, i+1);
            has_swapped = true;
        }
    }
    return has_swapped;
}

void bubble_sort(size_t n, long a[n]) {
    bool has_swapped = true;
    for (size_t last = n - 1; last > 0 && has_swapped; last -= 1) {
        has_swapped = bubble_pass(last, a);
    }
}
```
{% endcode %}

### Problem 22.2\*

Insertion sort by doing:

* take the first element X from the unsorted partition
* use binary search to find the correct position to insert X
* insert X into the right place

{% code lineNumbers="true" %}
```c
long search_loop(const long list[], long i, long j, long q) {
    
    while(i<=j) {
        long mid = (i+j)/2;
        if (list[mid] == q) {
            return mid;
        }
        if (list[mid] > q) {
            j = mid - 1;
            continue;
        }
        i = mid + 1;
    }
    return j;
}

void insert(long a[], size_t curr)
{
    long to_move = a[curr];
    size_t target = search_loop(a, 0, curr-1, to_move)+1;
    for(size_t i=curr; i > target; i--) {
        a[i] = a[i-1];
    }
    a[target] = to_move;
}

void insertion_sort(size_t n, long a[]) {
    for (size_t curr = 1; curr < n; curr += 1) {
        insert(a, curr);
    }
}
```
{% endcode %}

In Line 20, `curr-1` means we exclude our current number from the search range first. And the `+1` at last is because we want to insert at the position behind what we find using [#problem-21.2](tut-08-searching-and-sorting.md#problem-21.2 "mention")
