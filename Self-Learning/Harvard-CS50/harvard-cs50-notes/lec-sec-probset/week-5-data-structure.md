# Week 5 -Data Structure

Welcome to CS50! This is my review for Week 5's content.

## Lecture

1. Nothing much to note down.

## Section

1. There are two important parts in this line of code: `int *p = malloc(sizeof(int) * 4)`
   * `int *p`: This creates an int pointer on the **stack**
   * `malloc(sizeof(int) * 4)`: This allocates 4 integers on the **heap** and returns the address of the first integer. This address is then stored in the pointer `p`.
2. Using `free(p)`, you just free the memory on the **heap**, which means the pointer `p` is still on available on the stack and now it points to `NULL`.

## Shorts

### Singly Linked List

1. Define a linked list node using `struct`

{% code lineNumbers="true" %}
```c
typedef struct node
{
    int number;
    struct node *next;
}
node;
```
{% endcode %}

The `struct node` appearing twice here is important for the self-referencing.

2. Destroy the whole list using the idea of **recursion**:
   1. If you've reached a `NULL` pointer, return. (Base case)
   2. Delete the **rest of the list**
   3. Free the current node

### Tries

1. **Key-value** pairs:
   * In **Array**: The key is the **element index**, the value is **the data at that location**.
   * In **Hash Tables**: The key is the **hash code** of the data, the value is a **linked list of data hashing to that hash code** (supposed using chaining)
2.  In **Tries**, the data to be searched for is now a roadmap:

    * If you can follow the map from beginning to end, the data exists in the trie.
    * If you can't, it doesn't exist.
    * For example, using the idea of **key-value pair**, the keys are four-digit years(YYYY) and the values are names of universities founded during those years. In this trie, the paths from a central **root** node to a **leaf** node (where the school names would be), would be labeled with **digits of the year**. Each node on the path from root to leaf could have **10** pointers emanating from it, one for each digit. Its structure can be defined as below:

    {% code lineNumbers="true" %}
    ```c
    typedef struct _trie
    {
        char university[20];
        struct _trie *next[10];
    }
    trie;
    ```
    {% endcode %}

    * To search for an element in the trie, use successive digits to navigate from the root, and if you can make it to the end without hitting a dead end (a NULL pointer), you've found it. (This is similar to using the key to find the unique value)

## Problem Set 5

### [01 Inheritance](https://cs50.harvard.edu/x/2024/psets/5/inheritance/)

#### **Divide and Conquer**

1. `create_family()`

```
CreateFamily Algorithm:
1. Allocate memory for new person
2. If there are still generations left to create:
    a. Create two new parents for current person by recursively calling create_family
    b. Set parent pointers for current person
    c. Randomly assign current person's alleles based on the alleles of their parents
3. Else:
    a. Set parent pointers to NULL
    b. Randomly assign alleles
4. Return current person
```

2. `free_family()`

```
FreeFamily Algorithm:
1. Handle base case
2. Free parents recursively
3. Free child
```

### [02 Speller](https://cs50.harvard.edu/x/2024/psets/5/speller/)

#### **Things to notice in the problem statement**

1. The implementation of `check` must be **case-insensitive**.

#### **Divide and Conquer**

1. `check()`: Check whehter the word is in the dictionary

```
Check Algorithm:
1. Hash the word
2. Traverse the linked list at the hashed index
3. Compare the word with the words in the linked list
```

2. `hash()`: Hash the word to a number.

> Design a good hash function makes this problem interesting and here is my design, which will use the first **two** characters to determine the hash code of a word. To use the first two characters, we need to know the idea of **base-26**.

```
Hash Algorithm:
1. Set N to be 26 * 26
2. If the word has only one character:
    a. Return the uppercase of that character minus 'A'
3. Else:
    a. Return the uppercase of the first character minus 'A', then multiply by 26, and add the uppercase of the second character minus 'A'
4. Return the hash code
```

3. `load()`: Load the dictionary into memory.

```
Load Algorithm:
1. Open the dictionary file
2. While there are still words in the dictionary:
    a. Read the word
    b. Create a new node for the word
    c. Hash the word
    d. Insert the node into the linked list at the hashed index
3. Close the dictionary file
```

4. `size()`: Return the number of words in the dictionary

```
Size Algorithm:
1. Iterate through each element in the table
2. Iterate through the corresponding linked list
```

5. `unload()`: Unload the dictionary from memory

```
Unload Algorithm:
1. Iterate through each element in the table
2. Iterate through the corresponding linked list
3. Free the node using two node pointers
```

#### **Take-aways**

1. To read the word from the dictionary, we need to use [fscanf()](https://manual.cs50.io/3/fscanf), which is very useful to read from file besides `fread()`.
