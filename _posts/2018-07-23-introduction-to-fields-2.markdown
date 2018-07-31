---
layout: post
title:  "Introduction to Fields (Part 2)"
description: An introduction to finite fields.
date:   2018-07-23 18:00:00 -0400
categories:
tags: cryptography encryption math
---

<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

This post is an extension of [last week's post](/2018/07/10/introduction-to-fields.html) on basic field theory.
If you haven't read it yet, or aren't familiar with fields, I recommend taking a look at it before
reading this one.

## Review

Recall the definition of a field from last week:

**Definition:** We call a set $$ \mathbb{F} $$ a **field** if there are operators $$ + $$
(commonly called **addition**) and $$ \times $$ (commonly called **multiplication**)
such that all of the following properties are true:

1. $$ + $$ is **commutative**: $$ \; \forall x, y\in\mathbb{F}, \; x + y = y + x $$   
2. $$ + $$ is **associative**: $$ \;  \forall x, y, z\in\mathbb{F}, \; (x + y) + z = x + (y + z) $$    
3. existence of an **additive identity**: $$ \; \exists 0 \in\mathbb{F} \, $$ such that $$ \, \forall x\in\mathbb{F}, \; x + 0 = 0 + x = x \, $$
4. existence of an **additive inverse**: $$ \; \forall x \in\mathbb{F}, \; \exists (-x) \in \mathbb{F} \; $$
   such that $$ \, x + (-x) = -x + x = 0 $$
5. $$ \times $$ is **commutative**: $$ \; \forall x, y\in\mathbb{F}, \; x \times y = y \times x $$    
6. $$ \times $$ is **associative**: $$ \; \forall x, y, z\in\mathbb{F}, \; (x \times y) \times z = x \times (y \times z) $$    
7. **distributivity** of $$ \times $$ over $$ + $$: $$ \; \forall x, y, z \in \mathbb{F}, \; x \times (y + z) = x \times y + x \times z $$
8. existence of a **multiplicative identity**: $$ \; \exists 1 \in\mathbb{F} \, $$ such that
   $$ \, \forall x\in\mathbb{F}, \; x \neq 0, \; x \times 1 = 1 \times x = x $$
9. existence of an **multiplicative inverse**: $$ \; \forall x \in\mathbb{F}, \; x \neq 0, \; \exists x^{-1} \in\mathbb{F} \; $$
   such that $$ \, x \times x^{-1} = 1 $$

Where $$ x \times y $$ is often denoted as $$ x \cdot y $$, or $$ xy $$.

I also mentioned that the real numbers, $$ \mathbb{R} $$ and the rational numbers,
$$ \mathbb{Q} $$ are both fields, but the integers, $$ \mathbb{Z} $$ are not (because they do not
satisfy property **9** in the above definition).

Fantastic! So, with the review out of the way, lets get into this weeks content.

## Euclidean Algorithm

You may or may not have heard of the **Euclidean Algorithm**. This algorithm is a
method of computing the **Greatest Common Divisor (gcd)** of two integers.

Before we get into these definitions, I'm going to define some terminology that is pretty useful
when talking about division...

**Definition:** We say that "$$ d $$ **divides** $$ a $$" (denoted $$ d \mid a $$) if
$$ a = dk $$ for some $$ k \in \mathbb{Z} $$.

**Definition:** the **Greatest Common Divisor (gcd)** of $$ a, b \in \mathbb{Z} $$ is
equal to $$ d $$ if $$ d \mid a $$, $$ d \mid b $$ and if $$ \forall c \in \mathbb{Z} \, $$ such that
$$ \, c \mid a \, $$ and $$ \, c \mid b $$, then $$ c \leq d $$.

This is often denoted as $$ \textrm{gcd}(a, b) = d $$

The above definition is just a long-winded way of saying that $$ \textrm{gcd}(a, b) = d $$ if
$$ d $$ is the largest integer that divdes both $$ a $$ and $$ b $$. Also, note that $$ \textrm{gcd}(x, 0) = x $$.

Okay cool. So what's the Euclidean algorithm? Well...

**Theorem:** $$ \textrm{gcd}(a, b) = \textrm{gcd}(b, r)$$ where  $$ r $$ is the remainder
when $$ a $$ is divided by $$ b $$.

This is ultra nice because it's really easy to write a function to calculate the gcd of
$$ a $$ and $$ b \,$$:

{% highlight c %}
#include <stdlib.h>

int gcd(int a, int b) {
    if (b == 0) {
        return abs(a);
    }
    return gcd(b, a % b);
}
{% endhighlight %}

_Cool._ This is great, but is there a way to guarantee that the result of this algorithm is actually correct?
(Hint, there is!)

**Theorem (GCD Characterization Theorem)** if $$ d $$ is a positive common divisor of $$ a $$ and $$ b $$, and there exist integers
$$ x $$ and $$ y $$ such that $$ ax + by = d $$, then $$ d = \textrm{gcd}(a, b) $$.

### Example

Let's try and find $$ \textrm{gcd}(210, 123) $$ using the Euclidean Algorithm, and verify it with GCD Characterization
Theorem.

Using the Euclidean Algorithm gives the following reduction:

$$
    \begin{align}
    \textrm{Since} &\quad 210 = 123(1) + 87, &\; &\textrm{gcd}(210, 123) = \textrm{gcd}(123, 87) \\
    \textrm{Since} &\quad 123 = 87(1) + 36, &\; &\textrm{gcd}(123, 87) = \textrm{gcd}(87, 36) \\
    \textrm{Since} &\quad 87 = 36(2) + 15, &\; &\textrm{gcd}(87, 36) = \textrm{gcd}(36, 15) \\
    \textrm{Since} &\quad 36 = 15(2) + 6, &\; &\textrm{gcd}(36, 15) = \textrm{gcd}(15, 6) \\
    \textrm{Since} &\quad 15 = 6(2) + 3, &\; &\textrm{gcd}(15, 6) = \textrm{gcd}(6, 3) \\
    \textrm{Since} &\quad 6 = 3(2) + 0, &\; &\textrm{gcd}(6, 3) = \textrm{gcd}(3, 0) \\
    \end{align}
$$

This is nice, but it's kind of annoying trying to find $$ x $$ and $$ y $$ in the $$ ax + by = d $$ that
GCD Characterization Theorem requires. That's what the Extended Euclidean Algorithm is for though!

## Extended Euclidean Algorithm

This algorithm is a variation of the Euclidean Algorithm that will allow us to determine $$ \textrm{gcd}(a,b) $$,
as well as the values of $$ x $$ and $$ y $$ that satisfy GCD Characterization Theorem.

Begin with the following table:

$$
\begin{array}{|c|c|c|c|}
\hline
 x & y & r & q \\ \hline
 1 & 0 & a & 0 \\ \hline
 0 & 1 & b & 0 \\ \hline
\end{array}
$$

Where $$ a > b $$. We can then add successive rows to the table. For $$ i > 2 $$, we can define $$ q_{i} $$ as

$$
q_{i} = \left\lfloor\frac{r_{i-2}}{r_{i-1}}\right\rfloor
$$

Where $$ \lfloor x \rfloor $$ is the largest integer less than or equal to $$ x $$. We can then define
$$ x_{i} $$, $$ y_{i} $$ and $$ r_{i} $$ as:

$$
\begin{align}
x_{i} &= x_{i-2} - q_{i}(x_{i-1}) \\
y_{i} &= y_{i-2} - q_{i}(y_{i-1}) \\
r_{i} &= r_{i-2} - q_{i}(r_{i-1}) \\
\end{align}
$$

The algorithm stops when $$ r_{i} = 0 $$ for some $$ i $$. We use the $$ x $$ and $$ y $$ values
from the second last row in the table, and $$ r $$ in the second last row of the table is
$$ \textrm{gcd}(a,b) $$.

_Cool._ Now seems like a good time for an example though...

### Example

Let's find $$ \textrm{gcd}(210, 123) $$, and we'll see if we can verify our earlier result.

$$
\begin{array}{|c|c|c|c|}
\hline
x & y & r & q \\ \hline
1 & 0 & \ 210 \ & 0 \\ \hline
0 & 1 & \ 123 \ & 0 \\ \hline
\end{array}
$$

We're now going to add a third row, where:

$$
q_{3} = \left\lfloor\frac{r_{1}}{r_{2}}\right\rfloor = \left\lfloor\frac{210}{123}\right\rfloor = 1
$$

so,

$$
\begin{align}
x_{3} &= 1 - (1)(0) = 1 \\
y_{3} &= 0 - (1)(1) = -1 \\
r_{3} &= 210 - 1(123) = 87 \\
\end{align}
$$

Continuing the algorithm gives us the following table:

$$
\begin{array}{|c|c|c|c|}
\hline
 x & y & r & q \\ \hline
 \ 1 & 0 & \ 210 & \ 0 \\ \hline
 \ 0 & 1 & \ 123 & \ 0 \\ \hline
 \ 1 & -1 & \ 87 & \ 1 \\ \hline
 \ -1 & 2 & \ 36 & \ 1 \\ \hline
 \ 3 & -5 & \ 15 & \ 2 \\ \hline
 \ -7 & 12 & \ 6 & \ 2 \\ \hline
 \ 17 & -29 & \ 3 & \ 2 \\ \hline
  \ -41 & 12 & \ 0 & \ 2 \\ \hline
\end{array}
$$

Since $$ 17(210) + (-29)(123) = 3 $$, $$ 3 \mid 210 $$, and $$ 3 \mid 123 $$,
we can verify that $$ \textrm{gcd}(210, 123) = 3 $$!

Nice! So we're done. Don't worry if you found that hard to follow, what's important is that
you understand the process, not that you're able to just ram numbers into a formula.
If you made it to the bottom of this post, nice job! I'll see you next week for some more
fun math.
