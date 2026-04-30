# Week 6 - Python

Welcome to CS50 again! This blog will contain my review for Week 6's content about Python.

## Lecture

1. Nothing much to note down.

## Section

1. Nothing to note down. Try yourself and read the document is quite helpful.

## Shorts

1. Not much to note down. Just introduction to Python.

## Problem Set 6

### C-to-Python-Sentimental

Nothing to say

### DNA

#### **Divide and Conquer**

```
DNA Algorithm:
1. Check for command-line usage
2. Read database file into a variable
3. Read sequence file into a variable
4. Find longest match of each STR in DNA sequence
5. Check database for matching profiles
```

#### **Useful Snippets**

1. Check for command-line uasge

{% code lineNumbers="true" %}
```python
if not len(sys.argv) == 3:
    print("error")
    return
```
{% endcode %}

2. Read database file into a variable

{% code lineNumbers="true" %}
```python
rows = []
with open(sys.argv[1]) as file:
    reader = csv.DictReader(file)
    for row in reader:
        rows.append(row)
```
{% endcode %}

3. Read DNA sequence file into a variable

{% code lineNumbers="true" %}
```python
with open(sys.argv[2]) as file:
    sequence = file.read()
```
{% endcode %}

4. Find longest match of each STR in DNA sequence

{% code lineNumbers="true" %}
```python
result = {}
for STR in STRs:
    count = longest_match(sequence, STR)
    result[STR] = f"{count}"
```
{% endcode %}

5. Check database for matching profiles

{% code lineNumbers="true" %}
```python
for row in rows:
    name = row.pop("name")
    if row == result:
        print(name)
        break
else:
    print("No match.")
```
{% endcode %}

#### **Take-aways**

1. The command-line arguments are stored in `sys.argv`, which is a list. The first argument, which is `sys.argv[0]` is the script's name and the arguments after this are the actual arguments.
2. Using this code to read from the file,

{% code lineNumbers="true" %}
```python
with open("filename") as file:
    content = file.read()
```
{% endcode %}

{% hint style="success" %}
The variable `content` can be accessed outside the `with` statement.
{% endhint %}

3\. The `==` can be used to determine whether two dictionaries are equivalent or not. 4. To pop a specific _key:value_ pair from the dictionary,

{% code lineNumbers="true" %}
```python
temp = dict.pop(key)`.  
```
{% endcode %}

5. To add add a _key:value_ pair to the dictionary,

{% code lineNumbers="true" %}
```python
dict = {}
dict[key] = value
```
{% endcode %}

## Lecture - Week6.5

1. In **reinforcement learning**, if the agent does well, you give them a reward, otherwise, punish them.
