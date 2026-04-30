# Diagnostic Quiz

## Problems

### 5. printf() behavior

```c
printf("%.2f", 41); // This will print 0.0 and generates a warning
```

### 7. scanf() and fgets()

To read in a 6-letter module code (e.g., "CS1010") from the standard input? We can use

```c
char module_code[7];
scanf("%6s", module_code);
// or
char module_code[7];
fgets(module_code, 7, stdin);
```

### 8. Output Analysis

{% code lineNumbers="true" %}
```c
long x;
scanf("%ld", &x);
printf("You entered %ld\n", x);
scanf("%ld", &x);
printf("You entered %ld\n", x);
```
{% endcode %}

If the input is a floating point number, the first `scanf()` at Line 2 will consume the number **in front of the floating point** and store it in the variable `x`. And the `scanf()` at Line 4 will read nothing because `.`(char) doesn't match with `%ld`(long), so `x` will still be the same number and is printed out again.
