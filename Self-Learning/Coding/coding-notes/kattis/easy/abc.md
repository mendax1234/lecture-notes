# ABC

## Question

{% embed url="https://open.kattis.com/problems/abc" %}

## Solution

### Idea

**Time Complexity:** $$O(1)$$

This problem is not hard, it is a variant of counting sort. But since the size is small and fixed, so I didn't implement a sorting algorithm since it is tedious in C.

1. Sort the three numbers first
2. Then using char ('A', 'B' or 'C') readed from the input minus 'A' as the index to visit the sorted array.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/abc/abc.c" %}
