# Champernowne Verification

## Question

{% embed url="https://open.kattis.com/problems/champernowneverification" %}

## Solution

### Idea

Instead of taking the input as an integer, I take it as a string. So, this problem becomes a basic **string manipulation** problem.

1. The first 9 possible digits is easy to validate
2. To validate the 10th digit, you can utilise the `strcmp()` from the [`string.h`](../../baisc-knowledge/c/library.md#string.h) C Standard Libaray.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/champernowneverification/champernowneverification.c" %}
