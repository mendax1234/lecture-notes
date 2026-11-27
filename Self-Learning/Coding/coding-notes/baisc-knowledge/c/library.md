# Library

## CS1010 Library

The CS1010 input and output library can be found below:

{% @github-files/github-code-block url="https://github.com/nus-cs1010/libcs1010" %}

To use the library on your local pc, please follow this [guide](https://nus-cs1010.github.io/2425-s1/guides/library.html?h=cs1010#clearing-screen).

## C Standard Library

The best way to see how to use a standard libarary function in C is to use Linux Programmer's Manual. The command used is `man <function_name>`.

### Table

#### string.h

| Name                                                          | Description                                                                                                                                                | Return Values                                                                                                                                                                                                                                              |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `int strcmp(const char *s1, const char *s2);`                 | The `strcmp()` function compares the two strings `s1` and `s2`.                                                                                            | <ul><li>0, if <code>s1</code> and <code>s2</code> are equal</li><li>a negative value if <code>s1</code> is less than <code>s2</code></li><li>a positive value if <code>s1</code> is bigger than <code>s2</code></li></ul>                                  |
| `size_t strspn(const char *s, const char *accept);`           | The `strspn()` function calculates the length (in bytes) of the initial segment of `s` which consists entirely of bytes in `accept`.                       | The `strspn()` function returns the number of bytes in the initial segment of `s` which consist only of bytes from `accept`.                                                                                                                               |
| `size_t strcspn(const char *s, const char *reject);`          | The `strcspn()` function calculates the length of the initial segment of `s` which consists entirely of bytes not in `reject`.                             | The `strcspn()` function returns the number of bytes in the initial segment of `s` which are not in the string `reject`.                                                                                                                                   |
| `char *strstr(const char *haystack, const char *needle);`     | The `strstr()` function finds the first occurrence of the substring `needle` in the string `haystack`. The terminating null bytes ('\0') are not compared. | This function return a pointer to the beginning of the located substring, or NULL if the substring is not found.                                                                                                                                           |
| `char *strcasestr(const char *haystack, const char *needle);` | The `strcasestr()` function is like `strstr()`, but ignores the case of both arguments.                                                                    | This function return a pointer to the beginning of the located substring, or NULL if the substring is not found.                                                                                                                                           |
| `char *strchr(const char *s, int c);`                         | The `strchr()` function returns a pointer to the first occurrence of the character `c` in the string `s`.                                                  | The `strchr()` function returns a pointer to the matched character or NULL if the character is not found. The terminating null byte is considered part of the string, so that if c is specified as '\0', the function returns a pointer to the terminator. |
| `char *strrchr(const char *s, int c);`                        | The `strrchr()` function returns a pointer to the last occurrence of the character `c` in the string `s`.                                                  | The `strrchr()` function return a pointer to the matched character or NULL if the character is not found. The terminating null byte is considered part of the string, so that if c is specified as '\0', the function returns a pointer to the terminator. |

#### stdlib.h

| Name                    | Description                                                                                                                                                                                                                                                                                      | Return Values                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------- |
| `void free(void *ptr);` | The `free()` function frees the memory space pointed to by ptr, which must have been returned by a previous call to `malloc()`, `calloc()`, or `realloc()`. Otherwise, or if `free(ptr)` has already been called before, undefined behavior occurs. If `ptr` is NULL, no operation is performed. | The `free()` function returns no value. |
