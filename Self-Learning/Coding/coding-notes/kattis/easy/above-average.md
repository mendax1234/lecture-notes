# Above Average

## Question

{% embed url="https://open.kattis.com/problems/aboveaverage" %}

## Solution

### Idea

Maybe in C, the trickiest part is to deal with the input. The algorithm itself is not that difficult.

1. Calculate the average
2. Iterate through the list and calculate the percentage above the average

### Code

The code is long because I have used the [cs1010 library](https://github.com/nus-cs1010/libcs1010/blob/master/src/cs1010.c).

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/aboveaverage/aboveaverage.c" %}

## Take-away

### Print % in C

To print the character `%` in C immediately behind the number. We can use `printf()`

{% code lineNumbers="true" %}
```c
double a = 3.0;
printf("%.3lf%%\n", a);
```
{% endcode %}
