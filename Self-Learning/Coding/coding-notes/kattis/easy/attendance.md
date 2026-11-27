# Attendance

## Question

{% embed url="https://open.kattis.com/problems/attendance2" %}

## Solution

### Idea

This question is an array manipulation problem. The idea is easy:

1. First Iteration is to mark the attendance: we start from the second entry and if encounter "Present!", mark the previous name as `present`.
2. Second Iteration is to print the absence: we start from the first entry and if the name is not "Present!" and its absence is false, print out the absent student's name.

But the implementation may depends individually. Personally, I used `struct` in C to solve it.&#x20;

{% code lineNumbers="true" %}
```c
typedef struct student {
  char *name;
  bool present;
} student;

```
{% endcode %}

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/attendance2/attendance2.c" %}
