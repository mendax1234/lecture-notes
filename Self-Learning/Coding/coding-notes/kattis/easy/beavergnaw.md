# Beavergnaw

## Question

{% embed url="https://open.kattis.com/problems/beavergnaw" %}

## Solution

### Idea

This is purely a math problem, a math problem about the solid geometry. Quite boring since it took me a while to derive the math formula. The steps are summarised as follows:

1. Calculate the volumne of the bigger cylinder `total`, which is $$\pi*(\frac{D}{2})^2*D$$
2. Use `total` minus the input `V` to get the volume of the wood eaten by the beaver, $$\text{wood}=\text{total}-\text{v}$$

Now the trickiest thing begins, we need to deduct the two truncated cone. Here we use the $$\text{bigCone}-\text{smallCone}$$ to calculate. And we first consider the upper half.

Notice that for the upper half, the $$\text{bigCone}$$ is with both radius and height as $$\frac{D}{2}$$, while the $$\text{smallCone}$$ is with both radius and height as $$\frac{d}{2}$$. So, the volume for the upper truncated cone should be $$\frac{1}{3}(\pi(\frac{D}{2})^3-\pi(\frac{d}{2})^3)$$.

So, for the two truncated cones, their total volume should be $$\frac{2}{3}(\pi(\frac{D}{2})^3-\pi(\frac{d}{2})^3)$$, which is same as $$\frac{1}{3}(\pi(\frac{D}{2})^2\cdot D-\pi(\frac{d}{2})^2\cdot d)$$.

Notice that the volumne of two truncated cones plus the volume of the inner cylinder will be the volumne of $$\text{wood}$$. And the volumne of the cylinder is given by $$\pi(\frac{d}{2})^2\cdot d$$, which can be simplify to $$2\pi r^3,\text{where }r=\frac{d}{2}$$. (Now, you should be able to solve for the $$d$$). Manipulate the formula, we have $$\frac{2}{3}(2\pi r^3)=\text{wood}-\frac{1}{3}\pi(\frac{D}{2}^2)\cdot D$$

3. Now, let's define a new variable `bigCones`, whose value is $$\frac{1}{3}\pi(\frac{D}{2})^2\cdot D$$
4. Then, define another intermediate variable `shape`, whose value is $$\text{wood}-\text{bigCones}$$
5. Last and finally, define the last variable `cylinder` to represent the volume of the inner cylinder, whose value is $$\text{shape}\cdot \frac{3}{2}$$.&#x20;
6. Now, our $$d=2\cdot r=2\cdot\sqrt[3]{\frac{\text{cylinder}}{2\pi}}$$

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/beavergnaw/beavergnaw.c" %}
