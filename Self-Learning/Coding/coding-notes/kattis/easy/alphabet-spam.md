# Alphabet Spam

## Question

{% embed url="https://open.kattis.com/problems/alphabetspam" %}

## Solution

### Idea

The idea is quite simple. Loop through the string once and record the number of white spaces ('\_'), lowercase letters, uppercase letters and symbols.

The tricky part still lies in the input of the string. So, for simplicity, I just use the cs1010 library.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/alphabetspam/alphabetspam.c" %}
