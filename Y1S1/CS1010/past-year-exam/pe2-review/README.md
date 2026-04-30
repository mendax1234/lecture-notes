# PE2 Review

## Important Functions

### Array

#### Define a Fixed length 2-D array

{% code overflow="wrap" lineNumbers="true" %}
```c
long dir[8][2] = {{1, -1}, {1, 1}, {-1, 1}, {-1, -1}, {0, -1}, {0, 1}, {1, 0}, {-1, 0}};
```
{% endcode %}

### Numbers

#### is\_prime()

Return whether a number is a prime or not. Time complexity is $$O(\sqrt{N})$$.

{% code lineNumbers="true" %}
```c
bool is_prime(long num)
{
  for (long i = 2; i * i <= num; i += 1)
  {
    if (num % i == 0)
    {
      return false;
    }
  }
  return true;
}
```
{% endcode %}

#### count\_factors()

Return the number of prime factors of a num. Time complexity is $$O(\sqrt{N})$$

{% code lineNumbers="true" %}
```c
long is_semiprime(long num)
{
  long all = 0;
  long curr = num;
  for (long factor = 2; factor * factor <= curr && curr != 1; factor += 1) {
    long count = 0;
    while (curr % factor == 0) {
      curr = curr / factor;
      count += 1;
    }
    if (count != 0) {
      all += count;
    }
  }
  if (curr != 1) {
    all += 1;
  }
  return all;
}
```
{% endcode %}

### Strings

#### is\_match()

`char *word` is your source string, `size_t i` is the position in `word` you want to start searching for. `char *search_for` is the string that you want to search for in `word`.

<pre class="language-c"><code class="lang-c">/*
 * @param word       the word (can be a line) you want to search for
 * @param i          the position in the word to start searching
 * @param search_for the string you want to search for
 *
 * @output return true if search_for is in word. Otherwise return false
<strong> */
</strong>bool is_match(char *word, size_t i, char *search_for)
{
    size_t word_len = strlen(word);
    size_t search_len = strlen(search_for);
    size_t j = 0;
    for (j = 0; j &#x3C; search_len &#x26;&#x26; i + j &#x3C; word_len; j += 1) {
        if (word[i + j] != search_for[j] ) {
            return false;
        }
    }
    if (j == search_len) {
        return true;
    }
    return false;
}
</code></pre>

{% hint style="info" %}
The use of `i` here is awesome! We won't be afraid that our pointer to the string will be moved away!
{% endhint %}

#### substitute()

An intermediate function that replace the target string with the replace string plus white spaces.

{% code title="Substitute.c" overflow="wrap" lineNumbers="true" %}
```c
/*
 * @param word the source word (can be a line)
 * @param i    the current position
 * @param target_len the length of the target string (the string to be searched for)
 * @param replace_with the string to replace with
*/
void substitute(char *word, size_t i, size_t target_len, char *replace_with) {
    size_t replace_len = strlen(replace_with);
    size_t j = 0;
    while (j < replace_len) {
        word[i + j] = replace_with[j];
        j += 1;
    }
    while (j < target_len) {
        word[i + j] = ' ';
        j += 1;
    }
}
```
{% endcode %}

{% hint style="info" %}
Here it is assumed that the length of the target string is **longer than or equal to** the length of the replace string.
{% endhint %}

#### compact()

An intermediate function that takes in the source string `word` and reform the string by ignoring the white space in it.

{% code title="Compact.c" lineNumbers="true" %}
```c
void compact(char *word)
{
    // Use the same assumption above
    size_t src = 0;
    size_t dst = 0;
    size_t word_len = strlen(word);
    while (src < word_len) {
        if (word[src] != ' ') {
            word[dst] = word[src];
            dst += 1;
        }
        src += 1;
    }
    word[dst] = 0;
}
```
{% endcode %}

{% hint style="info" %}
Based on the same assumption above, our `dst` is always smaller than or equal to `src`
{% endhint %}

#### replace()

A complete function that uses [#is\_match](./#is_match "mention"), [#substitute](./#substitute "mention") and [#compact](./#compact "mention") above to achieve the purpose that after the execution of the function, all the `serach_for` substrings that have appeared in the source string will be replaced with the `replace_with` string.

{% code title="Replace.c" lineNumbers="true" %}
```c
/*
 * @param word the source string word (can be a line)
 * @param search_for the substring to be searched for (or target string)
 * @param replace_with the string used to replace the search_for/target string
*/
void replace(char *word, char *search_for, char *replace_with)
{
    size_t word_len = strlen(word);
    size_t search_len = strlen(search_for);
    for (size_t i = 0; i < word_len; i += 1) {
        if (is_match(word, i, search_for)) {
            substitute(word, i, search_len, replace_with);
            // Form a new "string" Awesome!
            i += search_len -­ 1;
        }
    }
    compact(word);
}
```
{% endcode %}

#### My Solution for [#id-5.-substring](pe2-ay20-21.md#id-5.-substring "mention")

{% code lineNumbers="true" %}
```c
/**
 * @param src  The source string
 * @param len  The length of source string
 * @param k    The length of the remaining characters
 * @param curr The position / Number of characters we have appended
 * @param sub  The substring
 */
void print_sub(char *src, size_t len, size_t k, size_t curr, char *sub)
{
  // No more remaining characters
  if (k == 0)
  {
    sub[curr] = '\0';
    cs1010_println_string(sub);
    return;
  }
  // Iterate through all the possible characters we can choose
  for (size_t i = 0; i <= len - k; i += 1)
  {
    sub[curr] = src[i];
    print_sub(src + i + 1, len - i - 1, k - 1, curr + 1, sub);
  }
}
```
{% endcode %}

### Binary Search

The classic one:

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

#### Binary Search for the position

Instead of returning -1 if the query `q` is not found, modify the binary search algorithm such that it returns either:

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

#### Binary Search in a Rotated Sorted List

> Appeared in [#id-3.-rotate](pe2-ay18-19.md#id-3.-rotate "mention")

{% code lineNumbers="true" %}
```c
/* 
 * @param a   the rotated sorted list
 * @param i   start position in the list (inclusive)
 * @param j   end position in the list (inclusive)
 * @param q   the number to search for
 * @param len the length of the list
 *
 * @output the position of q in the rotated sorted list
*/
long search(long *a, long i, long j, long q, size_t len)
{
  if (i > j)
  {
    return -1;
  }
  // Get how many bits have been right rotated
  long rotate = rotate_pos(a, len);
  long mid = (i + j) / 2;
  // Calculate the updated index
  long mid_rotated_pos = within_range(mid, rotate, len);
  if (a[mid_rotated_pos] == q)
  {
    return mid_rotated_pos;
  }
  if (a[mid_rotated_pos] > q)
  {
    return search(a, i, mid - 1, q, len);
  }
  return search(a, mid + 1, j, q, len);
}
```
{% endcode %}

Where, `within_range()` is implemented as follows;

{% code lineNumbers="true" %}
```c
long within_range(long index, long rotate, size_t len)
{
  if ((index + rotate) >= (long)len)
  {
    return index + rotate - (long)len;
  }
  return index + rotate;
}
```
{% endcode %}

#### Binary Search to get the length of "#"

> Appear in [#id-4.-box](pe2-ay21-22.md#id-4.-box "mention") (AY21/22) and [#id-4.-mode](pe2-ay23-24.md#id-4.-mode "mention")(AY23/24)

1. Recursive Method

{% code lineNumbers="true" %}
```c
/*
 * @param histogram The input board
 * @param start     The start searching position (inclusive)
 * @param end       The end searching position (exclusive)
 * @param row       The specific row to search for
 *
 * @output the number of '#' in the row
*/
size_t search_row(char **histogram, size_t start, size_t end, size_t row)
{
  if (end == start)
  {
    return start;
  }
  size_t mid = (start + end) / 2;
  if (histogram[row][mid] == '#')
  {
    return search_row(histogram, mid + 1, end, row);
  }
  return search_row(histogram, start, mid, row);
}
```
{% endcode %}

2. Iterative Method

{% code lineNumbers="true" %}
```c
/*
 * @param histogram The input board
 * @param r         The specific row to search for
 * @param m         The length of the row (exclusive)
*/
size_t score(char **histogram, size_t r, size_t m)
{
  size_t i = 0;
  size_t j = m - 1;
  while (i < j)
  {
    size_t mid = (i + j) / 2;
    if (histogram[r][mid] == '#' && histogram[r][mid+1] == '#')
    {
      i = mid+1;
    }
    else
    {
      j = mid;
    }
  }
  return i+1;
}
```
{% endcode %}

### Counting Sort

This is the **only** sorting algorithm that has the chance to take $$O(N)$$ to sort a list.

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

### Selection Sort

> Time complexity is $$O(N^2)$$

The soul of this algorithm is to 1) partition the list into two parts, an unsorted part followed by a sorted part, 2) repeatedly **selects the maximum element from the unsorted part** and **move it to the front of the sorted part**. (For increasing sort).

Otherwise, for decreasing sort, the idea will be to find the **smallest unsorted element** and add it to the **end of the sorted list.**

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

### Insertion Sort

Similar to [Selection Sort\*](https://wenbo-notes.gitbook.io/cs1010-notes/lec-tut-lab-exes/lecture/lec-09-searching-and-sorting#selection-sort), now we partition our list into 1) a sorted one, 2) followed by an unsorted one. Then we repeatedly take the first element from the **unsorted partition**, find its rightful place in the **sorted partition**, and _insert_ it into place. We start with a sorted partition of one element, and we end if the sorted partition contains all the elements.

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

#### Improve Insertion Sort using Binary Search

* Take the first element X from the unsorted partition
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

### Advanced Recursion

#### Permutation

{% code lineNumbers="true" %}
```c
/**
 * Fix a[0]..a[curr - 1] but permute characters a[curr]..a[len - 1]
 * Print out each permutation.
 *
 * @param[in,out] a    The array to permute
 * @param[in]     len  The size of the array
 * @param[in]     curr The starting index at which we will permute
 *
 * @post The string a remains unchanged
 */
void permute(char a[], size_t len, size_t curr) {
  if (curr == len - 1) {
    cs1010_println_string(a);
    return;
  }

  for (size_t i = curr; i < len; i += 1) {
    swap(a, curr, i);
    permute(a, len, curr + 1);
    swap(a, i, curr);
  }
}
```
{% endcode %}

#### NQueens - Find all solutions

{% code lineNumbers="true" %}
```c
/**
 * Search for all possible queens placement from row to n-1, 
 * given the queens placement from row 0 to row-1.
 *
 * @param[in] queens  The string representation of queens 
 *                    placement.
 * @param[in] n       The size of the chess board
 * @param[in] row     The last row where the queens positions 
 *                    have been fixed.
 */
void nqueens(char queens[], size_t n, size_t row) {
  if (row == n - 1) {
    if (!threaten_each_other_diagonally(queens, row)) {
      cs1010_println_string(queens);
    }
    return;
  }

  for (size_t i = row; i < n; i++) {
    swap(queens, row, i);
    nqueens(queens, n, row + 1);
    swap(queens, row, i);
  }
}
```
{% endcode %}

#### NQueens - Find one solution quicker

{% code lineNumbers="true" %}
```c
/**
 * Search for all possible queens placement from row to n-1, 
 * given the queens placement from row 0 to row-1.
 *
 * @param[in] queens  The string representation of queens 
 *                    placement.
 * @param[in] n       The size of the chess board
 * @param[in] row     The last row where the queens positions 
 *                    have been fixed.
 */
bool nqueens(char queens[], long n, long row) {
  if (row == n - 1) {
    if (!threaten_each_other_diagonally(queens, n - 1)) {
      cs1010_println_string(queens);
      return true;
    }
    return false;
  }

  for (long i = row; i < n; i++) {
    swap(queens, row, i);
    if (!threaten_each_other_diagonally(queens, row)) {
      if (nqueens(queens, n, row + 1)) {
        return true;
      }
    }
    swap(queens, row, i);
  }
  return false;
}
```
{% endcode %}



## Standard C Libaray

### ctype.h

|   Function name   |                 Description                 |
| :---------------: | :-----------------------------------------: |
| `isalpha(char c)` |    check whether `c` is alphabetic or not   |
| `isdigit(char c)` | check whether `c` is a decimal digit or not |
| `islower(char c)` |   check whether `c` is a lowercase letter   |
| `ispunct(char c)` |  check whether `c` is a punctuation or not  |
| `isupper(char c)` |   check whether `c` is an uppercase letter  |
| `tolower(char c)` |    convert uppercase letters to lowercase   |
| `toupper(char c)` |    convert lowercase letters to uppercase   |

**Punctuation letters:** This is a set of ``! " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ ` { | } ~``

## Tips

1. When doing string traversal, it is strongly recommended to use `strlen()` to get the length of the string, and then use a **for loop** to traverse through thes string, with the loop variable `i` indicating the current position of the character we have traversed!
2. Refer back to [Lab 09 - Backtracking](https://wenbo-notes.gitbook.io/cs1010-notes/lec-tut-lab-exes/lab/lab-09-backtracking) when doing the backtracking problems! Usually, an easier way to help you understand is to **draw a "tree"**.
3. The [#id-3.-group](pe2-ay22-23.md#id-3.-group "mention") provides an awesome idea to use array to store the previous states when doing backtracking!
4. When writing recursion in 2-D array, always think about how would you **not** enter an "infinite loop". This is usually down in your **condition to enter the recursion**. Some common methods are:
   1. mark the attendance of the "cell" you have updated. (a.k.a build an `unvisited_array`)
   2. there is some initial "constant" that you can compare to
5. When doing Backtracking/Recursion problems, always think about the three statements, they will help you form the solution: a)**Current Stage,** b)**Terminating Condition,** c)**State Transitions**. Anyway, the whole idea is to **think about how to do the backtracking! (State Transitions)**. This is usually done by:
   1. either redo the step you have done
   2. write a new value to the step you have done
6. Somtimes changing the **loop condition** will change the obvious time complexity, pay attention to the "hidden" time complexity and sometimes this kind of change can be utilised to make your code more effificient. See [#id-2.-sun](pe2-ay20-21.md#id-2.-sun "mention")
7. When you want to improve the complexity of **searching/sorting** problems,
   1. try thinking about how to **narrow your "searching area"**. This will be very useful!
   2. think about find the pattern, this will affect your algorithm greatly. See [exercise-6-searching-and-sorting.md](../../lec-tut-lab-exes/exercises/exercise-6-searching-and-sorting.md "mention")
8. The `strlen()` from C standard lib `<string.h>` will return **the number of characters in a string** (excluding the terminating `'\0'`).
9. Whenever you see the Time complexity is $$O(logN)$$, try to think of **binary search**.
10. When implementing the search algorithm, always keep in mind:

    1. How to define our search range (The use of index is inclusive or not)
    2. When to stop our searching (How to decide the search is successful or not)

    In the classic [#binary-search](./#binary-search "mention"), our search range is all the number bewteen `list[i]` and `list[j]` (both inclusive). And there are two cases we should stop our searching: 1) when there is no element in our search range, 2) when `list[mid]` is what we want to find.

## Valuable Problems from Past Year

Below are the valueable problems I think are important or classic

{% file src="../../.gitbook/assets/Cheatsheet_range (2).pdf" %}
Valueable Problems from Past Year
{% endfile %}

{% hint style="info" %}
Here the number refers to the exact pages in the corresponding past year papers' **comments** version.
{% endhint %}
