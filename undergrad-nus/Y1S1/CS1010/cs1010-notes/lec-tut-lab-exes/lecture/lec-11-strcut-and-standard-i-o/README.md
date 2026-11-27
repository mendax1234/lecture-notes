# Lec 11 - Strcut & Standard I/O

Slides:

{% embed url="https://nus-cs1010.github.io/2425-s1/slides/cs1010-lec11.html#1" %}
Lecture Slides
{% endembed %}

## Structure

### Declaration

Just like a variable, a `struct` has a scope, and it follows the same rule as the scope of a variable (i.e., it is valid within the block it is declared in). Unlike declaration of a variable, it is common to declare a `struct` in the **global scope**, i.e., outside of any function, so that it is usable within the whole program.

{% code lineNumbers="true" %}
```c
struct module {
  char *code;
  char *title;
  long mc;
}
```
{% endcode %}

### Initialization

Use the struct we have declared just now, here's an example to show the initialization of a struct.

{% code lineNumbers="true" %}
```c
struct module cs1010;
cs1010.code = "CS1010";
cs1010.title = "Programming Methodology";
cs1010.mc = 4;
```
{% endcode %}

### Assigning a Structure Variable

We can assign one structure variable to another

{% code lineNumbers="true" %}
```c
struct module cs1010s = cs1010;
```
{% endcode %}

This assignment statement above is equivalent to assigning each member of the `struct` individually.

### Declare Struct as a Type

Using `typedef` on `struct` frees us from typing the word `struct` every time. We can do so with either:

{% code lineNumbers="true" %}
```c
typedef struct module {
  char *code;
  char *title;
  long mc;
} module;
```
{% endcode %}

or

{% code lineNumbers="true" %}
```c
typedef struct {
  char *code;
  char *title;
  long mc;
} module;
```
{% endcode %}

In either case, we can just use `module` like any other type:

```c
module cs1010e;
```

## Standard I/O

### printf

`printf()` takes in a variable number of arguments. For the first argument, it should be a **format string containing one or more format specifiers**, like `%s`. And the general format for the **format modifier** is:

```
%[flags][field_width][.precision][length_modifier]specifier
```

The specifier controls the interpretation of the argument. `s` for string, `c` for character, `d` for integer (base 10), `f` for floating-point number, `p` for pointer (base 16). We can additionally prepend this with _length modifier_. `ld` for `long` integer, `lld` for `long long`, and `lf` for `double`.

To format the output, we can prepend it with a number to indicate its _field width_, or minimum space used when printing. E.g., `%3d` will pad the number printed with **space** if the number printed is less than 3 digits. Adding a _flag_ 0 in front, `%03d`, will pad the number with 0s if the number printed is less than 3 digits. For floating-point numbers, we can additionally control the _precision_, or the number of digits printed after the decimal point. e.g. `%3.4lf` will print a double to four decimal points. The first 3 indicates that if the whole floating point number (integer + floating parts + 1 for the `.`) is less than length 3, white spaces will be padded at front. Otherwise, nothing will be padded.

Some examples:

{% code lineNumbers="true" %}
```c
printf("%10.4lf\n", 10.0);
//   10.0000
printf("%3.4lf\n", 10.0);
//10.0000
printf("%3d\n", 10);
// 10
printf("%3d\n",10000);
//10000
```
{% endcode %}

#### Mismatch Number of Arguments

Since `printf` expects a variable number of arguments, you can pass it fewer arguments than expected and the code would still compile (with warnings). If you push ahead and run it anyway, `printf` will **start to fetch arguments from the stack**, pretending that it is there, causing weird things to happen.

#### Printing User Input

We should also never do this:

{% code lineNumbers="true" %}
```c
char *str = cs1010_read_word();
printf(str);
```
{% endcode %}

The reason is that we have no control over what the user would type as input: the user may type "%s" into the standard input, so the variable `str` now points to `%s`, which `printf` treats as a format modifier, and output the content of the stack! This is a huge security risk and is known as the [externally-controlled format string](http://cwe.mitre.org/data/definitions/134.html) vulnerability.

We should always print a string using:

{% code lineNumbers="true" %}
```c
printf("%s", str);
```
{% endcode %}

### scanf

{% hint style="info" %}
Rule of Thumb: Don't use `scanf()`. (Unless, you know **exactly** what you do.)
{% endhint %}

{% hint style="info" %}
Rule 1: `scanf()` is not for _reading_ input, it's for _parsing_ input.
{% endhint %}

Like `printf`, `scanf` takes in one or more arguments, with the first argument being a format string containing one or more format specifiers. The format specifier for `scanf` is simpler and has the following pattern:

```
%[*][field_width][length_modifier]specifier
```

For instance, to read an integer, a floating-point number, and a string of at most 10 characters,

{% code lineNumbers="true" %}
```c
long l;
double d;
char s[11];
scanf("%ld %lf %10s", &l, &d, s);
```
{% endcode %}

`scanf` scans the standard input, and tries to match it to the format specified. **The space in between the format specifier matches zero or more white spaces (space, tab, newline)**. Scanning stops when an input character does not match such a format character or when an input conversion fails.

{% hint style="info" %}
Return values of `scanf()`: On success, `scanf()` will return **the number of input items successfully matched and assigned**; this can be fewer than provided for, or even zero, in the event of an early matching failure.
{% endhint %}

Adding a `*` to the format modifier means that `scanf` should consume the inputs but not store them in any variables. This, combined with `%[` `]` is useful to clear any remaining data from the standard input. Its usage is shown as follows:

{% code lineNumbers="true" %}
```c
long a;
long result = scanf("%ld", &a);
if (result == 1) {
  printf("%ld", a);
} else {
  scanf("%*[^\n]");
}
```
{% endcode %}

#### Invalid Pointers

Since `scanf` expects the caller to pass in pointers to variables for it to store the results, we need to be careful about what we pass in. It is easy to pass in something like this:

{% code lineNumbers="true" %}
```c
long *a;
scanf("%ld", a);
```
{% endcode %}

The compiler **would not warn** us since the type matches perfectly. **The program may crash** since the pointer is not pointing to a valid memory location accessible by the program.

#### Buffer Overflow

When we use `scanf` to read a string, it keeps reading until it reaches space, and stores everything that it reads into an array. The problem here is that we do not know when it will stop reading, and therefore how big is the array that we need to allocate for the input!

```c
char name[10];
printf("What's your name?", name);
scanf("%s", name);
printf("Hello %s!\n", name);
```

As `%s` is for strings, this should work with any input:

```
$ ./example3
What's your name? Paul
Hello Paul!

$ ./example3
What's your name? Christopher-Joseph-Montgomery
Segmentation fault
```

Well, now we have a _buffer overflow_. You might get `Segmentation fault` on a Linux system, any other kind of crash, maybe even a "correctly" working program, because, once again, the program has **undefined behavior**.

{% hint style="warning" %}
Buffer overflows in `C`

A _buffer overflow_ is a specific kind of _undefined behavior_ resulting from a program that tries to write more data to an (array) variable than this variable can hold. Although this is _undefined_, in practice it will result in overwriting some _other_ data (that happens to be placed after the overflowed buffer in memory) and this can easily crash the program.

One particularly dangerous result of a buffer overflow is overwriting the _return address_ of a function. The return address is used when a function exits, to jump back to the calling function. Being able to overwrite this address ultimately means that a person with enough knowledge about the system can cause the running program to execute **any other code** supplied as input. This problem has led to many security vulnerabilities; imagine you can make for example a webserver written in `C` execute your own code by submitting a specially tailored request...
{% endhint %}

So, here's the next rule:

{% hint style="info" %}
Rule 2: `scanf()` can be _dangerous_ when used carelessly. Always use **field widths** with conversions that parse to a string (like `%s`).
{% endhint %}

The field width is a number preceeding the conversion specifier. It causes `scanf()` to consider a maximum number of characters from the input when parsing for this conversion. Let's demonstrate it in a fixed program:

{% code lineNumbers="true" %}
```c
#include <stdio.h>

int main(void)
{
    char name[40];
    printf("What's your name? ");
    scanf("%39s", name);
    printf("Hello %s!\n", name);
}
```
{% endcode %}

We also increased the buffer size, because there might be really long names.

There's an **important thing to notice**: Although our `name` has room for 40 characters, we instruct `scanf()` not to read more than 39. This is because a string in `C` always needs a `0` byte appended to mark the end. When `scanf()` is finished parsing into a string, it appends this byte automatically, and there must be space left for it.

So, this program is now safe from buffer overflows. Let's try something different:

```
$ ./example4
What's your name? Martin Brown
Hello Martin!
```

Well, that's ... outspoken. What happens here? Reading some `scanf()` manual, we would find that `%s` parses a _word_, not a _string_, for example I found the following wording:

> **s**: _Matches a sequence of non-white-space characters_

A _white-space_ in `C` is one of _space_, _tab_ (`\t`) or _newline_ (`\n`).

{% hint style="info" %}
Rule 3: Although `scanf()` format strings can look quite similar to `printf()` format strings, they often have slightly different semantics. (Make sure to **read the fine manual**)
{% endhint %}

The general problem with parsing "a string" from an input stream is: _Where does this string end?_ With `%s`, the answer is _at the next white-space_. If you want something different, you can use `%[`:

* `%[a-z]`: parse as long as the input characters are in the range `a` - `z`.
* `%[ny]`: parse as long as the input characters are `y` or `n`.
* `%[^.]`: The `^` _negates_ the list, so this means parse as long as there is no `.` in the input.

We could change the program, so anything until a _newline_ will be parsed into our string:

{% code lineNumbers="true" %}
```c
#include <stdio.h>

int main(void)
{
    char name[40];
    printf("What's your name? ");
    scanf("%39[^\n]", name);
    printf("Hello %s!\n", name);
}
```
{% endcode %}

It might get a bit frustrating, but this is again a program with possible _undefined behavior_, see what happens when we just press `Enter`:

```
$ ./example5
What's your name? 
Hello ÿ¦e!
```

Here's another sentence from a `scanf()` manual, from the section describing the `[` conversion:

> _The usual skip of leading white space is suppressed._

With many conversions, `scanf()` automatically skips _whitespace_ characters in the input, but with some, it doesn't. Here, our _newline_ from just pressing enter isn't skipped, and it doesn't match for our conversion that explicitly excludes _newlines_. The result is: `scanf()` doesn't parse anything, our `name` remains _uninitialized_.

One way around this is to _tell_ `scanf()` to skip whitespace: If the format string contains any whitespace, it matches any number of whitespace characters in the input, including no whitespace at all. Let's use this to skip whitespace the user might enter before entering his name:

{% code lineNumbers="true" %}
```c
#include <stdio.h>

int main(void)
{
    char name[40];
    printf("What's your name? ");
    scanf(" %39[^\n]", name);
    //     ^ note the space here, matching any whitespace
    printf("Hello %s!\n", name);
}
```
{% endcode %}

Yes, this program works and doesn't have any [_undefined behavior_](#user-content-fn-1)[^1]), but I guess you don't like very much that nothing at all happens when you just press enter, because `scanf()` is skipping it and continues to wait for input that can be matched.

#### Read a number with scanf

{% hint style="info" %}
Rule 4: `scanf()` is a _very powerful_ function. (and with great power comes great responsibility ...)
{% endhint %}

A lot of parsing work can be done with `scanf()` in a very _concise_ way, which can be very nice, but it also has many pitfalls and there are tasks (such as reading a line of input) that are much simpler to accomplish with a simpler function. Make sure you **understand the rules** presented here, and if in doubt, read the `scanf()` manual **precisely**.

That being said, here's an example on how to read a number with retries using `scanf()`:

{% code lineNumbers="true" %}
```c
#include <stdio.h>

int main(void)
{
    int a;
    int rc;
    printf("enter a number: ");
    while ((rc = scanf("%d", &a)) == 0)  // Neither success (1) nor EOF
    {
        // clear what is left, the * means only match and discard:
        scanf("%*[^\n]");
        // input was not a number, ask again:
        printf("enter a number: ");
    }
    if (rc == EOF)
    {
        printf("Nothing more to read - and no number found\n");
    }
    else
    {
        printf("You entered %d.\n", a);
    }
}
```
{% endcode %}

It's not as nice as the version [using](https://sekrit.de/webdocs/c/beginners-guide-away-from-scanf.html) `strtol()`, because there is no way to tell `scanf()` **not** to skip whitespace for `%d` -- so if you just hit `Enter`, it will still wait for your input -- but it works and it's a really short program.

### fgets

There are several functions in `C` for _reading_ input. Let's have a look at one that's probably most useful to you: `fgets()`.

`fgets()` does a simple thing, it reads up to a given maximum number of characters, but stops at a newline, which is read as well. In other words: _It reads a line of input._

This is the function signature:

{% code lineNumbers="true" %}
```c
char *fgets(char *str, int n, FILE *stream)
```
{% endcode %}

There are two very nice things about this function for what we want to do:

* The parameter for the maximum length accounts for the necessary `0` byte, so we can just pass the size of our variable.
* The return value is either a pointer to `str` or `NULL` if, for any reason, nothing was read.

So let's rewrite this program again:

{% code lineNumbers="true" %}
```c
#include <stdio.h>

int main(void)
{
    char name[40];
    printf("What's your name? ");
    if (fgets(name, 40, stdin))
    {
        printf("Hello %s!\n", name);
    }
}
```
{% endcode %}

I assure you this is safe, but it has a little flaw:

```
$ ./example7
What's your name? Bob
Hello Bob
!
```

Of course, this is because `fgets()` also reads the _newline_ character itself. But the fix is simple as well: We use `strcspn()` to get the index of the _newline_ character if there is one and overwrite it with `0`. `strcspn()` is declared in `string.h`, so we need a new `#include`:

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char name[40];
    printf("What's your name? ");
    if (fgets(name, 40, stdin))
    {
        name[strcspn(name, "\n")] = 0;
        printf("Hello %s!\n", name);
    }
}
```
{% endcode %}

{% hint style="info" %}
`size_t strcspn(const char *s, const char *reject);`

The strcspn() function calculates the length of the initial segment of `s` which consists entirely of bytes not in `reject`.
{% endhint %}

Let's test it:

```
$ ./example8
What's your name? Bob Belcher
Hello Bob Belcher!
```

## Credits

At last, I really want to thanks this amazing wesbite, it's legit awesome and useful!

{% embed url="https://sekrit.de/webdocs/c/beginners-guide-away-from-scanf.html" %}

[^1]: actually, this isn't even entirely true. This program _still_ has _undefined behavior_ for empty input. You could force this on a Linux console hitting `Ctrl+D` for example. So, it's again an example for _code you should not write_.
