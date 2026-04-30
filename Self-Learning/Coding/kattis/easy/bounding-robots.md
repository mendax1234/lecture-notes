# Bounding Robots

## Question

{% embed url="https://open.kattis.com/problems/boundingrobots" %}

## Solution

### Idea

This is a pretty good question to practice the introductory **decomposition** skills. This problem can be divided into two solvable smaller parts.

{% stepper %}
{% step %}
### Calculate the "Robot Thinking" Coordinates

This is quite simple, just add the input distance (should be properly converted to include the negative number) to the x- and y-coordinate.
{% endstep %}

{% step %}
### Calculate the Actual Coordinates

This should be done "step-by-step", which means once you give an input, you calculate the actual coordinates.

And the actual coordinates should be **within the bound**.
{% endstep %}
{% endstepper %}

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/boundingrobots/boundingrobots.c" %}
