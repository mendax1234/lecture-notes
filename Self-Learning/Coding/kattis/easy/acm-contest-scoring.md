# ACM Contest Scoring

## Question

{% embed url="https://open.kattis.com/problems/acm" %}

## Solution

### Idea

This is a quite interesting problem. Here we can build a frequency table to store the penalty for each question. (Use the hidden information that the maximum number of question is 26). And every time we encounter a **wrong**, just add 20 to the penalty of that question. Once we encounter a right, add the current time and the question's penalty time to the total time.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/acm/acm.c" %}
