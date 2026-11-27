# Week 4 - Memory

Welcome to CS50 again. This is Week 4 and I will go through my review of this week's content.

## Lecture

1. `malloc()` is just _memory allocate_, which will use the memory from the **heap**. And heap will grow downwards. There is also another section called **stack**, and **function calls and the variables** used will be stored in the stack. After the calling, the memory will be freed. Due to this reason, when we pass parameters by value, we just pass the copy and it won't affect the original value since after the function call, the memory will be freed. To affect the original value, we need to pass the address of the parameters.
2. Use the idea of point 1, we can understand the why in `scanf()`, we need to pass the **address** of the variable we want to store the input. That's because if we don't pass the address, we can't change the variable, which means we can't store our input into the variable. For example,

{% code lineNumbers="true" %}
```c
// n won't store the number input
int n;
scanf("%i", n);

// only when you pass in the address can you change the n
scanf("%i", &n);
```
{% endcode %}

To understand it, we can use _pass by reference_ when we want to change the parameters we passed in the function.

{% code lineNumbers="true" %}
````c
// The swap() that doesn't work
void swap(int a, int b)
{
    int tmp = a;
    a = b;
    b = tmp;
}
```
```c
// The swap() that works
void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}
````
{% endcode %}

3. When we pass the variable in the `scanf()`, the variable must be **initialized**. For data type like `int`, it's suffice to just declare the variable. But for data type like pointers, declaration only is not suffice, initialization is also needed. For example

{% code lineNumbers="true" %}
```c
// int type
int n;
scanf("%i", &n);

// pointers type - not working
char *p;
scanf("%s", p);

// working
char p[4];
scanf("%s", p);
```
{% endcode %}

For data type like `int`, it’s okay for you not to initialize since it always has **valid space**, so we can always rewrite the value in that valid space safely. But for pointers, not initializing is dangerous because what p stores is a **garbage value** (If you don’t initialize after declaring variables, what they have are all **garbage values**). This garbage value may represent an **invalid address** in the memory, so you can not write into that address.

4. When using `scanf()` to read string, scanf won't stop, even if you have specified the length of the string. This is dangerous. But what `get_string()` in `cs50.h` does is to use `malloc()` repeatedly to allocate more and more memory space to get the string.
5. In the lecture, I really apprecaite Prof David's explanation about `*`(dereferencing). It can be viewed as "go to", for example

{% code lineNumbers="true" %}
```c
int *p, n;
n = 1;
p = &n;
printf("%i", *p);
```
{% endcode %}

In line 4, `*p` can be viewed as going to the value store in `p`, which is an address and get the value stored in that address.

## Section

1. A pointer usually stores address. For example,

{% code lineNumbers="true" %}
```c
int n = 1;
int *p = &n;
```
{% endcode %}

In this code snippet, `p` is the name for the pointer and `int *` means the type is a pointer that points to `int`.

2. Some key syntax about pointer:
   1. **type \*** is a pointer that stores the address of a **type**&#x20;
   2. **\*x** takes a pointer **x** and gets the value stored at that address&#x20;
   3. **\&x** takes **x** and gets its address
3. `fread()` explanation\
   It takes four arguments, **To where (an address), what size (the size of a chunk), how many (how many chunkcs to read at a time), from where (an address)**. For example,&#x20;

{% code lineNumbers="true" %}
```c
 string filename = argv[1];
 FILE *f = fopen(filename, "r");
 uint8_t buffer[4];
 fread(buffer, 1, 4, f);
```
{% endcode %}

In this code snippet, the size of the chunk is one byte, and we want to read 4 chunks at a time from the file and read them to the `uint8_t` array called buffer.

## Shorts

### Pointers

1. In C, `NULL` pointer is the simplest pointer available to us. And the `NULL` pointer points to nothing.
2. When you create a pointer and you don't set its value immediately, you should **always** set the value of the pointer to **NULL**.
3. When you dereference a pointer that is **NULL**, you will get a **segmentation fault**.
4. To define three pointers using one line, you should write

{% code lineNumbers="true" %}
```c
// Correct way
int *a, *b, *c;
// Wrong way
int *a, b, c;
```
{% endcode %}

5. The size of a pointer is either $$4$$ or $$8$$ byte, depending on the system you are using. For example, in a $$32$$-bit system, the size of a pointer is $$4$$ byte, and in a $$64$$-bit system, the size of a pointer is $$8$$ byte.

### Defining Custom Data Types

1. In C, we use `typedef` to create a shorthand or rewritten name for data types. It's structure is below, which is very very important.

{% code lineNumbers="true" %}
```c
typedef <old name> <new name>
```
{% endcode %}

### Dynamic Memory Allocation

1. Statically and Dynamically

{% code lineNumbers="true" %}
```c
// statically obtain an integer
int x;

// dynamically obtain an integer
int *px = malloc(sizeof(int));
```
{% endcode %}

In this code, `x` is stored on the **stack**. The pointer variable `px` is also stored on the stack but the memeory it points to is a block of memory on the **heap**. To further understand this, see this example,

{% code lineNumbers="true" %}
```c
// array of floats on the stack
float stack_array[x];

// array of floats on the heap
float *heap_array = malloc(x * sizeof(float));
```
{% endcode %}

2. Dynamically-allocated memory is **not** automatically returned to the system after finishing using it, so we need to use `free()` to free the dynamically-allocated memory. Otherwise, there will be a **memory-leak**. However, statically-allocated memory doesn't have this issue since it's stored on the stack and the memory will be freed after the function call.

### Call Stack

1. Stack frame\
   When you call a function, the system sets aside space in memory for that function to do its necessary work.
   * We frequently call such chunk of memory **stack frames** or **function frames** (Not **stack only**).
2. More than one function's stack frame may exist in memory at a given time.
   * For example, if `main()` calls `move()`, which then calls `direction()`, all three functions have open frames.
   * These frames are arranged in a **stack**. The frame for the most-recently called function is always on the top of the stack.
   * When a new function is called, a new frame is **pushed** onto the top of the stack and becomes the active frame.
   * When a function finishes, its frame is **popped** off the stack, and the frame below it becomes the active frame again.

### File Pointers

1. `fopen()`

{% code lineNumbers="true" %}
```c
// Usage
FILE *ptr = fopen(<filename>, <operation>)

// Operation: read
FILE *ptr = fopen("file1.txt", "r");

// Operation: write
FILE *ptr = fopen("file2.txt", "w");

// Operation: append
FILE *ptr = fopen("file2.txt", "a");
```
{% endcode %}

{% hint style="info" %}
Notice that **write** means writing from the beginning, which will erase all the existing data in that file. While **append** means appending at the end of the file, which won't destroy anything in that file.
{% endhint %}

2\. `fclose()`

{% code lineNumbers="true" %}
```c
//Usage
fclose(<file pointer>);
```
{% endcode %}

3. `fgetc()`

{% code lineNumbers="true" %}
```c
// Usage
char ch = fgetc(<file pointer>)
```
{% endcode %}

{% hint style="info" %}
Notice that the file pointer must be **read** mode, otherwise, it will return an error.

* Using `fgetc()` to implement `cat` command in Linux, which will print the content of the file.

{% code lineNumbers="true" %}
```c
  char ch;
  while ((ch = fgetc(ptr)) != EOF)
      printf("%c", ch);
```
{% endcode %}
{% endhint %}

4\. `fputc()`

{% code lineNumbers="true" %}
```c
// Usage
fputc(<character>, <file pointer>)
```
{% endcode %}

{% hint style="info" %}
Notice that the file pointer must be **write** or **append** mode, otherwise, it will return an error.

* Using `fputc()` to implement `cp` command in Linux, which will copy the content of one file to another file.

```c
  char ch;
  while ((ch = fgetc(ptr1)) != EOF)
      fputc(ch, ptr2);
```
{% endhint %}

5\. `fread()`

{% code lineNumbers="true" %}
```c
// Usage
fread(<buffer>, <size>, <quantity>, <file pointer>)

// Example
double *arr2 = malloc(sizeof(double) * 80);
fread(arr2, sizeof(double), 80, ptr);
```
{% endcode %}

* Reads **quantity** number of **size** bytes from the file and stores them in the **buffer**.
* Notice that the file pointer must be **read** mode, otherwise, it will return an error.

6\. `fwrite()`

{% code lineNumbers="true" %}
```c
// Usage
fwrite(<buffer>, <size>, <quantity>, <file pointer>)

// Example
double *arr2 = malloc(sizeof(double) * 80);
fwrite(arr2, sizeof(double), 80, ptr);
```
{% endcode %}

* Writes **quantity** number of **size** bytes from the **buffer** to the file.
* Notice that the file pointer must be **write** or **append** mode, otherwise, it will return an error.

## Problem Set 4

### [01 Volume](https://cs50.harvard.edu/x/2024/psets/4/volume/)

#### **Things to notice in the problem statement**

1. The wav file has two parts, one is a 44 byte **header**, the other is the **sample** part.

#### **Divide and Conquer**

```
Algorithm: Volume

1. Copy header from input file to output file
2. Create a buffer for a single sample
   - Write updated data to output file

3. While reading a sample from the input file:
   - Update the sample
   - Write the updated sample to the output file
```

#### **Take-aways**

1. In `fopen()` and `fwrite()`, the opened file “remembers” the number of bytes that were successfully read, such that subsequent calls to this function for stream will return bytes after those already read.
2. `fopen()` and `fwrite()` will return the number of items read/written.

### [02 Filter - Easy](https://cs50.harvard.edu/x/2024/psets/4/filter/less/)

#### **Things to notice in the problem statement**

1. The bmp file is made up of pixels and each pixel is composed of 3 Bytes, one for Red, one for Blue and one for Green.

#### **Divide and Conquer**

1. `grayscale()`

```
Grayscale Algorithm:
1. Loop over all pixels
2. Take average of red, green, and blue
3. Update pixel values
```

2. `sepia()`

```
Sepia Algorithm:
1. Loop over all pixels
2. Compute sepia values
3. Update pixel with sepia values
```

3. `reflect()`

```
Reflect Algorithm:
1. Loop over all pixels in the left side
2. Swap pixels
```

4. `blur()`

```
Blur Algorithm:
1. Create a copy of image
2. Loop over all pixels
3. Compute average of surrounding pixels using copied image
4. Update pixel with average
```

#### **Useful Snippets**

1. `reflect()`

{% code lineNumbers="true" %}
```c
// Reflect image horizontally
    void reflect(int height, int width, RGBTRIPLE image[height][width])
    {
        // Loop over the left side of pixels
        for (int i = 0; i < height; i++)
        {
            for (int j = 0; j < width / 2; j++)
            {
                // Swap pixels
                RGBTRIPLE temp;
                temp = image[i][j];
                image[i][j] = image[i][width - j - 1];
                image[i][width - j - 1] = temp;
            }
        }
        return;
    }
```
{% endcode %}

{% hint style="info" %}
Notice that we only need to iterate over the left side of the pixels. And we **cannot** let j to be **<=** `width / 2` since one pixel will be swapped twice as the index starts from 0. Also, the index of the corresponding right pixel is needs another minus 1 from `width - j`.
{% endhint %}

2\. `blur()`

{% code lineNumbers="true" %}
```c
// Blur image
void blur(int height, int width, RGBTRIPLE image[height][width])
{
    // Create a copy of image
    RGBTRIPLE copy[height][width];
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            copy[i][j] = image[i][j];
        }
    }

    // Blur
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            // Calculate blur
            double times = 0;
            double sumRed = 0;
            double sumBlue = 0;
            double sumGreen = 0;
            for (int p = i - 1; p <= i + 1; p++)
            {
                for (int q = j - 1; q <= j + 1; q++)
                {
                    if (p >= 0 && p < height && q >= 0 && q < width)
                    {
                        times++;
                        sumRed += copy[p][q].rgbtRed;
                        sumBlue += copy[p][q].rgbtBlue;
                        sumGreen += copy[p][q].rgbtGreen;
                    }
                }
            }
            double averageRed = round(sumRed / times);
            double averageBlue = round(sumBlue / times);
            double averageGreen = round(sumGreen / times);

            // Update pixels
            image[i][j].rgbtRed = (int) averageRed;
            image[i][j].rgbtBlue = (int) averageBlue;
            image[i][j].rgbtGreen = (int) averageGreen;
        }
    }
    return;
}
```
{% endcode %}

{% hint style="info" %}
Notice that the most consice way to calculate the blur is to use a nested loop as shown in the code snippet above. Also, since we will update our image, we need to create a copy of the original image to calculate the blur. Otherwise, we may run into the resource conflict.
{% endhint %}

### [02 Filter - Hard](https://cs50.harvard.edu/x/2024/psets/4/filter/more/)

**Things to notice in the problem statement**

1. The first three functions (`grayscale()`, `reflect()` and `blur()`) are the same as the previous question. So, in this part, I will only talk about the `edge()`.

#### **Divide and Conquer**

1. `edge()`

```
Edge Algorithm:
1. Create a copy of image
2. Create a 3x3 array for Gx and Gy
3. Loop over all pixels
4. Compute Gx and Gy
5. Compute G from Gx and Gy
6. Update pixel with G
```

#### **Useful Snippet**

1. `edge()`

{% code lineNumbers="true" %}
```c
// Detect edges
void edges(int height, int width, RGBTRIPLE image[height][width])
{
    // Create a copy of image
    RGBTRIPLE copy[height][width];
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            copy[i][j] = image[i][j];
        }
    }

    
    // Create Gx and Gy
    int Gx[3][3] = {{-1, 0, 1}, {-2, 0, 2}, {-1, 0, 1}};
    int Gy[3][3] = {{-1, -2, -1}, {0, 0, 0}, {1, 2, 1}};
    

    // Procedure
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            // Calculate Gx and Gy
            double sumGxRed = 0;
            double sumGyRed = 0;
            double sumGxBlue = 0;
            double sumGyBlue = 0;
            double sumGxGreen = 0;
            double sumGyGreen = 0;
            for (int p = i - 1; p <= i + 1; p++)
            {
                for (int q = j - 1; q <= j + 1; q++)
                {
                    // Index for accessing Gx and Gy
                    int x = p - (i - 1);
                    int y = q - (j - 1);

                    // Calculate the sumGx and sumGy rgb
                    if (p >= 0 && p < height && q >= 0 && q < width)
                    {
                        sumGxRed += copy[p][q].rgbtRed * Gx[x][y];
                        sumGyRed += copy[p][q].rgbtRed * Gy[x][y];
                        sumGxBlue += copy[p][q].rgbtBlue * Gx[x][y];
                        sumGyBlue += copy[p][q].rgbtBlue * Gy[x][y];
                        sumGxGreen += copy[p][q].rgbtGreen * Gx[x][y];
                        sumGyGreen += copy[p][q].rgbtGreen * Gy[x][y];
                    }
                }
            }
            double finalRed = round(sqrt(pow(sumGxRed, 2) + pow(sumGyRed, 2)));
            double finalBlue = round(sqrt(pow(sumGxBlue, 2) + pow(sumGyBlue, 2)));
            double finalGreen = round(sqrt(pow(sumGxGreen, 2) + pow(sumGyGreen, 2)));

            // Update pixels
            image[i][j].rgbtRed = (finalRed > 255.0) ? 255 : (int) finalRed;
            image[i][j].rgbtBlue = (finalBlue > 255.0) ? 255 : (int) finalBlue;
            image[i][j].rgbtGreen = (finalGreen > 255.0) ? 255 : (int) finalGreen;
        }
    }
    return;
}
```
{% endcode %}

{% hint style="info" %}
Notice that to use initializer to initialize a multi-dimension array, we need to use `{}` to enclose each row.
{% endhint %}

### [03 Recover](https://cs50.harvard.edu/x/2024/psets/4/recover/)

#### **Things to notice in the problem statement**

1. Each JPEG starts with a distinct header
   * First three bytes: `0xff`, `0xd8`, `0xff`
   * Last Byte: `0xe0` to `0xef`
2. JPEGs stored back-to-back in memory card

#### **Divide and Conquer**

```
Recover Algorithm:
1. Open Memory Card
2. Repeat until end of card:
    a. If start of a new JPEG:
        i. If it's the first JPEG:
            - Do some initial operation
        ii. Else:
            - Close the previous file
            - Do some operation
    b. Else:
        i. If already found a JPEG:
            - Write the block to the file
        ii. Else:
            - Continue
3. Close any remaining files
```

#### **Take-aways**

1. To make sure a byte start with `e` in this question, we can use the idea of **bitwise operation** to achieve this. In this problem, `(buffer[3] & 0xf0) == 0xe0` can serve this purpose very well.
2. The File pointer (`FILE *`) is also allocated on the **heap**, so at the end of the program, remember to close the files you have opened. In this problem, don't forget to close the `card.raw`.
