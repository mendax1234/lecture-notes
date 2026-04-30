# ASCII kassi

## Question

{% embed url="https://open.kattis.com/problems/asciikassi" %}

## Solution

### Idea

This is a problem of **pattern recognition** and **breaking big problems into smaller ones to solve**. Based on our observation, the first pattern we've found is that the top and bottom line are the same and the middle lines follow the same pattern. So, we can break our problems in `print_top_bottom()` and `print_mid()`.

In `print_top_bottom()`, we may find that the start and end are the same, which is `+`. While the middle part is `-` and its number is equal to the input N, which is the length of your square.

In `print_mid()`, firstly, we find that we need to print exactly N lines, and in each line, the start and the end are `|`, while the middle are N of  (white space).

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/asciikassi/asciikassi.c" %}
