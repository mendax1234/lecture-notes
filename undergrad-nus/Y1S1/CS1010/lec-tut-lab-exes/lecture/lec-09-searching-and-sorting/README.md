# Lec 09 - Searching and Sorting

Slides:

{% embed url="https://nus-cs1010.github.io/2425-s1/slides/cs1010-lec09.html#1" %}
Lecture Slides
{% endembed %}

## Searching

### Linear Search

Given that a list is **unsorted**, as we have seen in [tut-02.md](../../tutorial/tut-02.md "mention"), whatever method we choose, it is impossible for us "skip" any element, we must check each element in the list to safely say whether our query is in the list or not.

So, Linear Search is actually a **brute-force** approach that does the search by iterating through each element once in the list.

### Binary Search\*

However, if our list is **sorted**, we can improve our algorithm greatly beacuse there is a "pattern" in our list. In this case, the pattern is **a sorted list**. Based on this pattern, we can implement our Bineary Search algorithm as follows:

{% code lineNumbers="true" %}
```c
/**
 * Look for q in list[i]..list[j].
 *
 * @pre list is sorted
 *
 * @para list[] is the list given
 * @para i      is the starting index (inclusive)
 * @para j      is the ending index (inclusive)
 * @para q      is our query number
 *
 * @return -1 if not found, the position of q in list otherwise.
 */
long search(const long list[], long i, long j, long q) {
  if (i > j) {
    return -1;
  }
  long mid = (i+j)/2;
  if (list[mid] == q) {
    return mid;
  } 
  if (list[mid] > q) {
    return search(list, i, mid-1, q);
  } 
  return search(list, mid+1, j, q);
}
```
{% endcode %}

The main idea in this algorithm is that it repeatedly **cuts the range of values to search by half**.

{% hint style="info" %}
When implementing the search algorithm, always keep in mind:

1. How to define our search range (The use of index is inclusive or not)
2. When to stop our searching (How to decide the search is successful or not)

In this binary search algorithm, our search range is all the number bewteen `list[i]` and `list[j]` (both inclusive). And there are two cases we should stop our searching: 1) when there is no element in our search range, 2) when `list[mid]` is what we want to find.
{% endhint %}

The idea of **binary serach** is practiced in [#id-4.-box](../../../past-year-exam/pe2-review/pe2-ay21-22.md#id-4.-box "mention"). And the idea of **cutting the search range** is practiced in [#id-4.-soil](../../../past-year-exam/pe2-review/pe2-ay20-21.md#id-4.-soil "mention").

## Sorting

Eldon's lecture about the cost of swap is really important. Take the real world considerations into algorithm design account.

Also, the procedure to derive the complexity is interesting and there may have some places to take note of during the derivation!

### Counting Sort\*

> This is the **only** sorting algorithm that has the chance to take $$O(n)$$ to sort a list.

The soul of this algorithm is to build a "frequency table" and then use this table to construct the sorted result.

{% code lineNumbers="true" %}
```c
/**
 * Perform counting sort on the input in[] and store the sorted
 * numbers in out[].
 *
 * @param[in] in The array containing numbers to be sorted.
 * @param[out] out The array containing the sorted numbers.
 * @param[in] len The size of the input and output array.
 *
 * @pre in[i] is between 0 and MAX for all i.
 * @post out[] is sorted
 */
void counting_sort(size_t len, const long in[], long out[])
{
  size_t freq[MAX + 1] = { 0 };

  for (size_t i = 0; i < len; i += 1) {
    freq[in[i]] += 1;
  }

  size_t outpos = 0;
  for (long i = 0; i <= MAX; i += 1) {
    for (size_t j = outpos; j < outpos + freq[i]; j += 1) {
      out[j] = i;
    }
    outpos += freq[i];
  }
}
```
{% endcode %}

### Selection Sort\*

> The time complexity is $$O(n^2)$$

The soul of this algorithm is to 1) partition the list into two parts, an unsorted part followed by a sorted part, 2) repeatedly **selects the maximum element from the unsorted part** and **move it to the front of the sorted part**. (For increasing sort).

{% hint style="info" %}
Otherwise, for decreasing sort, the idea will be to find the **smallest unsorted element** and add it to the **end of the sorted list.**
{% endhint %}

{% code lineNumbers="true" %}
```c
/**
 * Find the index of the largest element among list[0..last].
 *
 * @param[in] last The last element to search.
 * @param[in] list Input list
 * @return The index of the max element among list[0..last].
 *         Breaking ties by choosing the smaller index.
 * @pre list is not NULL and list[0] .. list[last] are valid.
 */
size_t max(size_t last, const long list[])
{
  long max_so_far = list[0];
  size_t max_index = 0;
  for (size_t i = 1; i <= last; i += 1) {
    if (list[i] > max_so_far) {
      max_so_far = list[i];
      max_index = i;
    }
  }
  return max_index;
}

/**
 * Sort a list using selection sort.
 *
 * @param[in] n The size of the list to sort.
 * @param[in] list The input list
 * @pre list is not NULL and list[0]..list[n-1] are valid
 * @post The list is sorted.
 */
void selection_sort(size_t n, long list[])
{
  for (size_t j = n - 1; j >= 1; j -= 1) {
    size_t max_pos = max(j, list);
    swap(list, max_pos, j);
  }
}
```
{% endcode %}

### Bubble Sort\*

> The time complexity is $$O(n^2)$$

The idea is that this algorithm sorts the largest number to the end until the beginning by a series of **bubble pass**. In each pass, we look for all possible adjacent pairs of items. Any adjacent pair that is out of order is swapped so that they are in order. So, after each pass, we will **move the largest number to its correct position** and in the next pass, we will exclude it.

{% code lineNumbers="true" %}
```c
void bubble_pass(size_t last, long a[]) {
  for (size_t i = 0; i < last; i += 1) {
    if (a[i] > a[i+1]) {
      swap(a, i, i+1);
    }
  }
}

void bubble_sort(size_t n, long a[n]) {
  for (size_t last = n - 1; last > 0; last -= 1) {
    bubble_pass(last, a);
  }
}
```
{% endcode %}

### Insertion Sort\*

Similar to [#selection-sort](./#selection-sort "mention"), now we partition our list into 1) a sorted one, 2) followed by an unsorted one. Then we repeatedly take the first element from the **unsorted partition**, find its rightful place in the **sorted partition**, and _insert_ it into place. We start with a sorted partition of one element, and we end if the sorted partition contains all the elements.

{% code lineNumbers="true" %}
```c
void insert(long a[], size_t curr)
{
  size_t i = curr;
  long temp = a[curr]; // temp stores the element we want to insert
  while (i >= 1 && temp < a[i - 1]) {
    a[i] = a[i - 1];
    i -= 1;
  }
  // i represents the correct position we will insert into
  a[i] = temp;
}

void insertion_sort(size_t n, long a[]) {
  for (size_t curr = 1; curr < n; curr += 1) {
    insert(a, curr);
  }
}
```
{% endcode %}

## Tips

1. One thing to note is that every time you see time complexity $$O(\log n)$$ in the question, try to think of binary search.
2.  When implementing the search algorithm, always keep in mind:

    1. How to define our search range (The use of index is inclusive or not)
    2. When to stop our searching (How to decide the search is successful or not)

    In this binary search algorithm, our search range is all the number bewteen `list[i]` and `list[j]` (both inclusive). And there are two cases we should stop our searching: 1) when there is no element in our search range, 2) when `list[mid]` is what we want to find.
