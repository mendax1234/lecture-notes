# Cetiri

## Question

{% embed url="https://open.kattis.com/problems/cetiri" %}

## Solution

### Idea

This is basically a **math** problem.

1. Sort the array using any sorting algorithm you like
2. **Find the correct common difference:** This is done by first finding the difference between **first two** elements (`first_diff`) and the difference between the **last two** elements (`last_diff`). Note that either one of them will be the **correct** common difference for this arithmetic progression.
   1. If `first_diff == last_diff`, that means both of them are the correct common difference.
   2. If `first_diff > last_diff`, that means the **second** element is missing, and it should be the **third** element **minus** `last_diff`, which is the correct common difference.
   3. If `first_diff < last_diff`, that means the **third** element is missing, and it should be the **second** element **plus** the `first_diff`, which is the correct common difference.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/cetiri/cetiri.c" %}
