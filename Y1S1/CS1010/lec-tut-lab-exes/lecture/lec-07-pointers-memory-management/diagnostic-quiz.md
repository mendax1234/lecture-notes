# Diagnostic Quiz

## Problems

### 6. Pointer Usage

Remember the pointer type match we have discussed in the lecture. See [#type-match](./#type-match "mention").

* A pointer to `<type>` can only store the address of the variable with the same `<type>`
* You cannot assign `double` to a `long` variable, this will generate compilation error!

### 13. Memory Leak

1. free a "heap-object" pointer twice will cause **undefined behavior** or **memory error**
2. after freeing a "heap-object" pointer, if you access the address pointed to by the pointer, you will get **memory error**.

### 15. Valid String Declaration

In C, only the characters enclosed by `""` will be considered as a **valid** string.

```c
// Valid
char *s = "I am the real C string."

// Invalid
char *s = """
I am a C string.
"""
char *s = 'I am a string too!'
char s[] = { 's', 't', 'r', 'i', 'n', 'g' };
```

